<template>
    <section class="post-details mt-4 pb-5" v-if="design != null">
        <div class="container-xl">
            <div class="row mt-4">
                <div class="col-12 col-md-8">
                    <div class="post-detail">
                        <h2 class="h2 mb-4">{{ design.title }}</h2>
                        <div class="single-img">
                            <img
                                :src="design.images['original']"
                                alt="Image"
                                class="img-fluid" />
                        </div>
                    </div>
                    <div class="desing-text fs-5 fw-light py-3 pt-2">
                        <p>
                            {{ design.description }}
                        </p>
                    </div>
                    <!-- for mobile -->
                    <div class="col-12 col-md-4 d-block d-md-none">
                        <div class="post-detail-sidebar">
                            <div class="card" style="border: 0">
                                <div class="card-body pt-0">
                                    <div class="row align-items-center">
                                        <div class="col-6">
                                            <div class="row">
                                                <div class="col-3">
                                                    <img
                                                        :src="
                                                            design.user
                                                                .profile_image
                                                        "
                                                        alt="User Image"
                                                        class="img-fluid rounded-circle" />
                                                </div>
                                                <div class="col-9">
                                                    <RouterLink
                                                        :to="{
                                                            name: 'user-details',
                                                            params: {
                                                                id: design.user
                                                                    .id,
                                                            },
                                                        }">
                                                        <h5
                                                            class="card-title mb-1">
                                                            {{
                                                                design.user.name
                                                            }}
                                                        </h5>
                                                    </RouterLink>
                                                    <p class="card-text">
                                                        {{
                                                            design.user
                                                                .job_title
                                                        }}
                                                    </p>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-6">
                                            <p class="fw-light text-end mb-0">
                                                {{ design.created_at }}
                                            </p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <ul class="details-side-meta fs-6 fw-light px-3">
                                <li
                                    class="d-table w-100 text-black"
                                    style="padding-bottom: 0">
                                    <div class="stats-txt d-table-cell w-50">
                                        <a
                                            href=""
                                            @click.prevent="like(design.id)"
                                            class="text-black"
                                            v-if="design.liked">
                                            <span v-if="!isLoadingLike">
                                                <i
                                                    class="bi-heart-fill text-danger"></i>
                                            </span>
                                            <span v-if="isLoadingLike">
                                                <!-- By Sam Herbert (@sherb), for everyone. More @ http://goo.gl/7AJzbL -->
                                                <svg
                                                    width="38"
                                                    height="38"
                                                    viewBox="0 0 38 38"
                                                    xmlns="http://www.w3.org/2000/svg">
                                                    <defs>
                                                        <linearGradient
                                                            x1="8.042%"
                                                            y1="0%"
                                                            x2="65.682%"
                                                            y2="23.865%"
                                                            id="a">
                                                            <stop
                                                                stop-color="#000"
                                                                stop-opacity="0"
                                                                offset="0%" />
                                                            <stop
                                                                stop-color="#000"
                                                                stop-opacity=".631"
                                                                offset="63.146%" />
                                                            <stop
                                                                stop-color="#000"
                                                                offset="100%" />
                                                        </linearGradient>
                                                    </defs>
                                                    <g
                                                        fill="none"
                                                        fill-rule="evenodd">
                                                        <g
                                                            transform="translate(1 1)">
                                                            <path
                                                                d="M36 18c0-9.94-8.06-18-18-18"
                                                                id="Oval-2"
                                                                stroke="url(#a)"
                                                                stroke-width="2">
                                                                <animateTransform
                                                                    attributeName="transform"
                                                                    type="rotate"
                                                                    from="0 18 18"
                                                                    to="360 18 18"
                                                                    dur="0.9s"
                                                                    repeatCount="indefinite" />
                                                            </path>
                                                            <circle
                                                                fill="#000"
                                                                cx="36"
                                                                cy="18"
                                                                r="1">
                                                                <animateTransform
                                                                    attributeName="transform"
                                                                    type="rotate"
                                                                    from="0 18 18"
                                                                    to="360 18 18"
                                                                    dur="0.9s"
                                                                    repeatCount="indefinite" />
                                                            </circle>
                                                        </g>
                                                    </g>
                                                </svg>
                                            </span>
                                            Unlike
                                        </a>
                                        <a
                                            href=""
                                            @click.prevent="like(design.id)"
                                            class="text-black"
                                            v-else>
                                            <span v-if="!isLoadingLike">
                                                <i class="bi-heart"></i>
                                            </span>
                                            <span v-if="isLoadingLike">
                                                <!-- By Sam Herbert (@sherb), for everyone. More @ http://goo.gl/7AJzbL -->
                                                <svg
                                                    width="38"
                                                    height="38"
                                                    viewBox="0 0 38 38"
                                                    xmlns="http://www.w3.org/2000/svg">
                                                    <defs>
                                                        <linearGradient
                                                            x1="8.042%"
                                                            y1="0%"
                                                            x2="65.682%"
                                                            y2="23.865%"
                                                            id="a">
                                                            <stop
                                                                stop-color="#000"
                                                                stop-opacity="0"
                                                                offset="0%" />
                                                            <stop
                                                                stop-color="#000"
                                                                stop-opacity=".631"
                                                                offset="63.146%" />
                                                            <stop
                                                                stop-color="#000"
                                                                offset="100%" />
                                                        </linearGradient>
                                                    </defs>
                                                    <g
                                                        fill="none"
                                                        fill-rule="evenodd">
                                                        <g
                                                            transform="translate(1 1)">
                                                            <path
                                                                d="M36 18c0-9.94-8.06-18-18-18"
                                                                id="Oval-2"
                                                                stroke="url(#a)"
                                                                stroke-width="2">
                                                                <animateTransform
                                                                    attributeName="transform"
                                                                    type="rotate"
                                                                    from="0 18 18"
                                                                    to="360 18 18"
                                                                    dur="0.9s"
                                                                    repeatCount="indefinite" />
                                                            </path>
                                                            <circle
                                                                fill="#000"
                                                                cx="36"
                                                                cy="18"
                                                                r="1">
                                                                <animateTransform
                                                                    attributeName="transform"
                                                                    type="rotate"
                                                                    from="0 18 18"
                                                                    to="360 18 18"
                                                                    dur="0.9s"
                                                                    repeatCount="indefinite" />
                                                            </circle>
                                                        </g>
                                                    </g>
                                                </svg>
                                            </span>
                                            Like
                                        </a>
                                    </div>
                                    <div
                                        class="stats-num d-table-cell w-50 text-end">
                                        <p>{{ design.likes }} Likes</p>
                                    </div>
                                </li>
                            </ul>
                            <div class="designs-tag-outer mt-3">
                                <h2 class="fs-6 fw-normal mb-2 mx-3">Tags: </h2>
                                <span class="designs-tag fs-5 mx-3">
                                    <RouterLink
                                        :to="{
                                            name: 'home',
                                            query: { tags: tag },
                                        }"
                                        :title="tag"
                                        v-for="tag in design.tags_list.tag"
                                        :key="tag"
                                        >{{ tag }}
                                    </RouterLink>
                                </span>
                            </div>
                        </div>
                    </div>
                    <div class="design-comments mt-3">
                        <h1 class="fs-5 fw-light mb-4">
                            <strong class="fw-normal"
                                >({{ design.comments?.length ?? 0 }})
                                Responses</strong
                            >
                        </h1>
                        <div
                            v-if="authStore.isLoggedIn"
                            class="p-3 border-0"
                            style="background-color: #f8f9fa">
                            <form action="" @submit.prevent="post_comment">
                                <div class="d-flex flex-start w-100">
                                    <img
                                        class="rounded-circle shadow-1-strong me-3"
                                        :src="design.user.profile_image"
                                        alt="avatar"
                                        width="40"
                                        height="40" />
                                    <div class="form-outline w-100">
                                        <textarea
                                            v-model="comment_form.body"
                                            class="form-control"
                                            placeholder="Write a comment"
                                            rows="4"
                                            style="background: #fff"></textarea>
                                    </div>
                                </div>
                                <div class="pt-3 d-flex justify-content-end">
                                    <button
                                        type="submit"
                                        class="btn btn-primary btn-sm"
                                        :disabled="isLoading"
                                        :class="{ disabled: isLoading }">
                                        Post
                                    </button>
                                </div>
                            </form>
                        </div>
                        <ul class="list-unstyled comment-list">
                            <li
                                class="clearfix"
                                v-for="comment in design.comments">
                                <div class="comment-thumb float-start">
                                    <RouterLink
                                        :to="{
                                            name: 'user-details',
                                            params: { id: comment.user.id },
                                        }">
                                        <img
                                            :src="comment.user.profile_image"
                                            class="neba img-fluid" />
                                    </RouterLink>
                                </div>
                                <div class="comment-meta">
                                    <h3 class="fs-5 fw-normal mb-2">
                                        <RouterLink
                                            :to="{
                                                name: 'user-details',
                                                params: { id: comment.user.id },
                                            }"
                                            :title="comment.user.name"
                                            >{{ comment.user.name }}
                                        </RouterLink>
                                    </h3>
                                    <p class="fs-6 mb-2">
                                        {{ comment.body }}
                                    </p>
                                    <span class="fs-7 fw-light">
                                        {{ comment.created_at_human }}
                                    </span>
                                </div>
                                <div class="d-flex justify-content-end">
                                    <button
                                        class="btn btn-danger btn-sm"
                                        v-if="
                                            comment.user.id ===
                                            authStore.user?.id
                                        "
                                        @click="
                                            delete_comment($event, comment.id)
                                        ">
                                        Delete
                                    </button>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="col-12 col-md-4 d-none d-md-block">
                    <div class="post-detail-sidebar">
                        <div class="card" style="border: 0">
                            <div class="card-body pt-0">
                                <div class="row">
                                    <div class="col-md-4">
                                        <img
                                            :src="design.user.profile_image"
                                            alt="User Image"
                                            class="img-fluid rounded-circle mt-1" />
                                    </div>
                                    <div class="col-md-8">
                                        <RouterLink
                                            :to="{
                                                name: 'user-details',
                                                params: { id: design.user.id },
                                            }">
                                            <h5 class="card-title mb-1">
                                                {{ design.user.name }}
                                            </h5>
                                        </RouterLink>
                                        <p class="card-text">
                                            {{ design.user.job_title }}
                                        </p>
                                    </div>
                                </div>
                                <div class="rwo">
                                    <p class="fw-light mt-4">
                                        {{ design.created_at }}
                                    </p>
                                </div>
                            </div>
                        </div>
                        <ul class="details-side-meta fs-6 fw-light px-3">
                            <li
                                class="d-table w-100 text-black"
                                style="padding-bottom: 0">
                                <div class="stats-txt d-table-cell w-50">
                                    <a
                                        href=""
                                        @click.prevent="like(design.id)"
                                        class="text-black"
                                        v-if="design.liked">
                                        <span v-if="!isLoadingLike">
                                            <i
                                                class="bi-heart-fill text-danger"></i>
                                        </span>
                                        <span v-if="isLoadingLike">
                                            <!-- By Sam Herbert (@sherb), for everyone. More @ http://goo.gl/7AJzbL -->
                                            <svg
                                                width="38"
                                                height="38"
                                                viewBox="0 0 38 38"
                                                xmlns="http://www.w3.org/2000/svg">
                                                <defs>
                                                    <linearGradient
                                                        x1="8.042%"
                                                        y1="0%"
                                                        x2="65.682%"
                                                        y2="23.865%"
                                                        id="a">
                                                        <stop
                                                            stop-color="#000"
                                                            stop-opacity="0"
                                                            offset="0%" />
                                                        <stop
                                                            stop-color="#000"
                                                            stop-opacity=".631"
                                                            offset="63.146%" />
                                                        <stop
                                                            stop-color="#000"
                                                            offset="100%" />
                                                    </linearGradient>
                                                </defs>
                                                <g
                                                    fill="none"
                                                    fill-rule="evenodd">
                                                    <g
                                                        transform="translate(1 1)">
                                                        <path
                                                            d="M36 18c0-9.94-8.06-18-18-18"
                                                            id="Oval-2"
                                                            stroke="url(#a)"
                                                            stroke-width="2">
                                                            <animateTransform
                                                                attributeName="transform"
                                                                type="rotate"
                                                                from="0 18 18"
                                                                to="360 18 18"
                                                                dur="0.9s"
                                                                repeatCount="indefinite" />
                                                        </path>
                                                        <circle
                                                            fill="#000"
                                                            cx="36"
                                                            cy="18"
                                                            r="1">
                                                            <animateTransform
                                                                attributeName="transform"
                                                                type="rotate"
                                                                from="0 18 18"
                                                                to="360 18 18"
                                                                dur="0.9s"
                                                                repeatCount="indefinite" />
                                                        </circle>
                                                    </g>
                                                </g>
                                            </svg>
                                        </span>
                                        Unlike
                                    </a>
                                    <a
                                        href=""
                                        @click.prevent="like(design.id)"
                                        class="text-black"
                                        v-else>
                                        <span v-if="!isLoadingLike">
                                            <i class="bi-heart"></i>
                                        </span>
                                        <span v-if="isLoadingLike">
                                            <!-- By Sam Herbert (@sherb), for everyone. More @ http://goo.gl/7AJzbL -->
                                            <svg
                                                width="38"
                                                height="38"
                                                viewBox="0 0 38 38"
                                                xmlns="http://www.w3.org/2000/svg">
                                                <defs>
                                                    <linearGradient
                                                        x1="8.042%"
                                                        y1="0%"
                                                        x2="65.682%"
                                                        y2="23.865%"
                                                        id="a">
                                                        <stop
                                                            stop-color="#000"
                                                            stop-opacity="0"
                                                            offset="0%" />
                                                        <stop
                                                            stop-color="#000"
                                                            stop-opacity=".631"
                                                            offset="63.146%" />
                                                        <stop
                                                            stop-color="#000"
                                                            offset="100%" />
                                                    </linearGradient>
                                                </defs>
                                                <g
                                                    fill="none"
                                                    fill-rule="evenodd">
                                                    <g
                                                        transform="translate(1 1)">
                                                        <path
                                                            d="M36 18c0-9.94-8.06-18-18-18"
                                                            id="Oval-2"
                                                            stroke="url(#a)"
                                                            stroke-width="2">
                                                            <animateTransform
                                                                attributeName="transform"
                                                                type="rotate"
                                                                from="0 18 18"
                                                                to="360 18 18"
                                                                dur="0.9s"
                                                                repeatCount="indefinite" />
                                                        </path>
                                                        <circle
                                                            fill="#000"
                                                            cx="36"
                                                            cy="18"
                                                            r="1">
                                                            <animateTransform
                                                                attributeName="transform"
                                                                type="rotate"
                                                                from="0 18 18"
                                                                to="360 18 18"
                                                                dur="0.9s"
                                                                repeatCount="indefinite" />
                                                        </circle>
                                                    </g>
                                                </g>
                                            </svg>
                                        </span>
                                        Like
                                    </a>
                                </div>
                                <div
                                    class="stats-num d-table-cell w-50 text-end">
                                    <p>{{ design.likes }} Likes</p>
                                </div>
                            </li>
                        </ul>
                        <div class="designs-tag-outer mt-3">
                            <h2 class="fs-6 fw-normal mb-2 mx-3">Tags</h2>
                            <div class="designs-tag fs-5 mx-3">
                                <RouterLink
                                    :to="{ name: 'home', query: { tags: tag } }"
                                    :title="tag"
                                    v-for="tag in design.tags_list.tag"
                                    :key="tag"
                                    >{{ tag }}
                                </RouterLink>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <section v-else>
        <div id="loading">
            <svg
                width="38"
                height="38"
                stroke="#000"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg">
                <g>
                    <circle
                        cx="12"
                        cy="12"
                        r="9.5"
                        fill="none"
                        stroke-width="3"
                        stroke-linecap="round">
                        <animate
                            attributeName="stroke-dasharray"
                            dur="1.5s"
                            calcMode="spline"
                            values="0 150;42 150;42 150;42 150"
                            keyTimes="0;0.475;0.95;1"
                            keySplines="0.42,0,0.58,1;0.42,0,0.58,1;0.42,0,0.58,1"
                            repeatCount="indefinite" />
                        <animate
                            attributeName="stroke-dashoffset"
                            dur="1.5s"
                            calcMode="spline"
                            values="0;-16;-59;-59"
                            keyTimes="0;0.475;0.95;1"
                            keySplines="0.42,0,0.58,1;0.42,0,0.58,1;0.42,0,0.58,1"
                            repeatCount="indefinite" />
                    </circle>
                    <animateTransform
                        attributeName="transform"
                        type="rotate"
                        dur="1s"
                        values="0 12 12;360 12 12"
                        repeatCount="indefinite" />
                </g>
            </svg>
        </div>
    </section>
    <Teleport to="body">
        <LoginRequiredModal :show="showModal" @close="showModal = false" />
    </Teleport>
</template>

<script setup>
    import { useRoute } from "vue-router";
    import { onMounted, ref } from "vue";
    import { request } from "../../helpers/request";
    import { like_design } from "../../helpers/design";
    import { useAuthStore } from "../../stores/auth";
    import LoginRequiredModal from "../../components/LoginRequiredModal.vue";

    const route = useRoute();
    const authStore = useAuthStore();

    const design = ref(null);
    const showModal = ref(false);

    const comment_form = ref({
        body: null,
    });

    const isLoading = ref(false);
    const isLoadingLike = ref(false);

    onMounted(async () => {
        try {
            const res = await request("/api/designs/slug/" + route.params.slug);
            const data = await res.json();

            design.value = data.data;
        } catch (error) {
            console.error(error);
        }
    });

    async function like(design_id) {
        if (authStore.isLoggedIn) {
            isLoadingLike.value = true;
            try {
                await like_design(design_id);
                const res = await request(
                    "/api/designs/slug/" + route.params.slug
                );
                const data = await res.json();

                design.value = data.data;
            } catch (error) {
                console.error(error);
            }
            isLoadingLike.value = false;
        } else {
            showModal.value = true;
        }
    }

    async function post_comment() {
        isLoading.value = true;
        try {
            const res = await request(
                "/api/designs/" + design.value.id + "/comments",
                {
                    method: "POST",
                    body: JSON.stringify(comment_form.value),
                }
            );
            const data = await res.json();

            if (res.ok) {
                design.value.comments.unshift(data.data);
                comment_form.value.body = null;
            }
        } catch (error) {
            console.error(error);
        }
        isLoading.value = false;
    }

    async function delete_comment(e, comment_id) {
        e.target.disabled = true;
        e.target.classList.add("disabled");
        try {
            const res = await request(
                "/api/designs/" + design.value.id + "/comments/" + comment_id,
                {
                    method: "DELETE",
                }
            );
            const data = await res.json();

            if (res.ok) {
                design.value.comments = design.value.comments.filter(
                    comment => comment.id !== comment_id
                );
            }
        } catch (error) {
            console.error(error);
        }
        e.target.disabled = false;
        e.target.classList.remove("disabled");
    }
</script>

<style lang="scss" scoped>
    #loading {
        background-color: white;
        text-align: center;
        margin-top: 70px;
        margin-bottom: 70px;
        margin-right: auto;
        margin-left: auto;
        text-align: center;
        width: 100%;
    }
</style>
