<template>
    <div class="container-xl mt-4" id="my-designs">
        <div class="card">
            <h2 class="font-24 mt-4 mb-2 mx-3 fw-700">Designs</h2>
            <div class="card-body">
                <div id="wrapper"></div>
            </div>
        </div>
    </div>
</template>

<script setup>
    import { onBeforeMount, ref, watch } from "vue";
    import { request, getCSRFToken } from "../../helpers/request";
    import { delete_design } from "../../helpers/design";
    import { Grid, h } from "gridjs";
    import { useRoute, useRouter } from "vue-router";
    import { useToast } from "vue-toastification";
    import "gridjs/dist/theme/mermaid.css";
    import "vue-toastification/dist/index.css";

    const route = useRoute();
    const router = useRouter();
    const loading = ref(false);
    const toast = useToast();

    let grid = null;
    let default_search_q = route.query?.q ?? null;
    let reset_page = false;
    let max_page_nbr = 1;
    let delete_request = false;

    if (route.query.success) {
        toast.success("Design Updated Successfuly", {
            timeout: 3000,
        });
    }

    if (route.query.error) {
        toast.error("Something Went Wrong", {
            timeout: 3000,
        });
    }

    onBeforeMount(async () => {
        const csrfToken = await getCSRFToken();

        grid = new Grid({
            columns: [
                {
                    id: "thumbnail",
                    name: "",
                    minWidth: "105px",
                    formatter: cell => {
                        return h("img", {
                            src: cell,
                            alt: "design image",
                            style: `
                                    width: 100%;
                                    height: 100%;
                                    object-fit: cover;
                                `,
                        });
                    },
                },
                "Title",
                { name: "Status", sort: false, minWidth: "105px" },
                { name: "Likes", minWidth: "105px" },
                { name: "Tags", minWidth: "105px" },
                {
                    name: "Actions",
                    id: "id",
                    sort: false,
                    minWidth: "170px",
                    formatter: cell => {
                        return [
                            h(
                                "button",
                                {
                                    className: "btn btn-primary btn-sm",
                                    onClick: () =>
                                        router.push({
                                            name: "designs-edit",
                                            params: { id: cell },
                                        }),
                                },
                                "Edit"
                            ),
                            h(
                                "button",
                                {
                                    className: "btn btn-danger btn-sm mx-2",
                                    onClick: () => init_delete(cell),
                                },
                                "Delete"
                            ),
                        ];
                    },
                },
                { name: "id", hidden: true },
            ],
            search: {
                keyword: default_search_q,
                server: {
                    url: (prev, keyword) => {
                        if (default_search_q) {
                            default_search_q = null;
                        } else {
                            reset_page = true;
                        }

                        if (prev.indexOf("?") === -1) {
                            router.replace(`?q=${keyword}`);
                            return `${prev}?q=${keyword}`;
                        } else {
                            router.replace(`&q=${keyword}`);
                            return `${prev}&q=${keyword}`;
                        }
                    },
                },
            },
            pagination: {
                limit: 10,
                server: {
                    url: (prev, page, limit) => {
                        const qIndex = prev.indexOf("?");
                        let n_page;

                        if (qIndex === -1) {
                            if (reset_page) {
                                n_page = 1;
                                page = 1;
                            } else {
                                n_page = page + 1;
                            }
                            reset_page = false;

                            router.replace(`?page=${n_page}`);
                            return `${prev}?page=${n_page}`;
                        } else {
                            if (reset_page) {
                                n_page = 1;
                                page = 1;
                            } else {
                                n_page = page + 1;
                            }

                            reset_page = false;

                            router.replace(
                                `${prev.slice(qIndex)}&page=${n_page}`
                            );
                            return `${prev}&page=${n_page}`;
                        }
                    },
                },
                page: route.query?.page ? parseInt(route.query.page) - 1 : 0,
            },
            sort: {
                multiColumn: false,
                server: {
                    url: (prev, columns) => {
                        if (!columns.length) return prev;

                        if (columns[0]?.index === 0) return prev;

                        const col = columns[0];
                        const dir = col.direction === 1 ? "asc" : "desc";
                        let colName = [
                            "thumbnail",
                            "title",
                            "status",
                            "likes",
                            "actions",
                        ][col.index];

                        if (prev.indexOf("?") === -1) {
                            reset_page = true;
                            return `${prev}?order=${colName}&dir=${dir}`;
                        } else {
                            reset_page = true;
                            return `${prev}&order=${colName}&dir=${dir}`;
                        }
                    },
                },
            },
            server: {
                headers: {
                    "content-Type": "application/json",
                    accept: "application/json",
                    "X-XSRF-TOKEN": csrfToken,
                },
                credentials: "include",
                url: "http://localhost:8000/api/users/designs",
                then: data => {
                    if (data.data?.length === 1) {
                        max_page_nbr = parseInt(data.meta.last_page) - 1;
                    } else {
                        max_page_nbr = parseInt(data.meta.last_page);
                    }
                    return data.data.map(design => {
                        return {
                            thumbnail: design.images["thumbnail"],
                            title: design.title,
                            status: design.is_live ? "Published" : "Draft",
                            likes: design.likes,
                            tags: design.tags_list.tag.join(","),
                            id: design.id,
                        };
                    });
                },
                total: data => data.meta.total,
            },
            style: {
                td: {
                    padding: "8px",
                    verticalAlign: "middle",
                },
                th: {
                    paddingLeft: "8px",
                },
                table: {
                    minWidth: "100%",
                },
            },
        });

        grid.render(document.getElementById("wrapper"));
    });
    async function refreshData() {
        const csrfToken = await getCSRFToken();

        grid.updateConfig({
            pagination: {
                limit: 10,
                server: {
                    url: (prev, page, limit) => {
                        const qIndex = prev.indexOf("?");
                        let n_page;

                        if (qIndex === -1) {
                            if (page + 1 > max_page_nbr) {
                                page = max_page_nbr - 1;
                            }

                            n_page = page + 1;

                            router.replace(`?page=${n_page}`);
                            return `${prev}?page=${n_page}`;
                        } else {
                            if (page + 1 > max_page_nbr) {
                                page = max_page_nbr - 1;
                            }

                            n_page = page + 1;

                            router.replace(
                                `${prev.slice(qIndex)}&page=${n_page}`
                            );
                            return `${prev}&page=${n_page}`;
                        }
                    },
                },
                page: route.query?.page
                    ? parseInt(route.query.page) > max_page_nbr
                        ? max_page_nbr - 1
                        : parseInt(route.query.page) - 1
                    : 0,
            },
            server: {
                headers: {
                    "content-Type": "application/json",
                    accept: "application/json",
                    "X-XSRF-TOKEN": csrfToken,
                },
                credentials: "include",
                url: "http://localhost:8000/api/users/designs",
                then: data => {
                    if (data.data?.length === 1 && delete_request) {
                        max_page_nbr =
                            parseInt(data.meta.last_page) > 1
                                ? parseInt(data.meta.last_page) - 1
                                : 1;
                    } else {
                        max_page_nbr = parseInt(data.meta.last_page);
                    }

                    delete_request = false;

                    return data.data.map(design => {
                        return {
                            thumbnail: design.images["thumbnail"],
                            title: design.title,
                            status: design.is_live ? "Published" : "Draft",
                            likes: design.likes,
                            tags: design.tags_list.tag.join(","),
                            id: design.id,
                        };
                    });
                },
                total: data => data.meta.total,
            },
            search: {
                keyword: route.query?.q ?? null,
                server: {
                    url: (prev, keyword) => {
                        if (default_search_q) {
                            default_search_q = null;
                        } else {
                            reset_page = true;
                        }

                        if (delete_request) {
                            reset_page = false;
                        }

                        if (prev.indexOf("?") === -1) {
                            router.replace(`?q=${keyword}`);
                            return `${prev}?q=${keyword}`;
                        } else {
                            router.replace(`&q=${keyword}`);
                            return `${prev}&q=${keyword}`;
                        }
                    },
                },
            },
        }).forceRender();
    }

    async function init_delete(design_id) {
        if (confirm("are you sure ?")) {
            loading.value = true;
            const res = await delete_design(design_id);

            if (res.ok) {
                delete_request = true;
            }

            await refreshData();
            loading.value = false;
        }
    }

    watch(loading, (newVal, oldVal) => {
        if (newVal === true) {
            const container = document.querySelector(
                ".gridjs.gridjs-container"
            );
            container.classList.add("gridjs-loading");
            container.insertAdjacentHTML(
                "afterbegin",
                `<div class="gridjs-loading-bar"></div>`
            );
        } else {
            document
                .querySelector(".gridjs.gridjs-container")
                .classList.remove("gridjs-loading");
        }
    });
</script>

<style lang="scss" scoped>
    #my-designs {
        #wrapper .gridjs-loading {
            .disabled_gridjs_tr_sort {
                pointer-events: none;
            }
            .gridjs-table,
            .gridjs-wrapper {
                min-height: 200px;
            }
            .gridjs-thead {
                height: 50px;
            }
            .gridjs-tbody {
                height: 147px;
            }
        }
    }
</style>
