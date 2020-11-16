<template>
    <Layout>

        <!-- Page Header -->
        <header class="masthead"
                :style="{
                    backgroundImage: `url(${ GRIDSOME_API_URL + $page.general.edges[0].node.cover.url})`
                }">
            <div class="overlay"></div>
            <div class="container">
                <div class="row">
                    <div class="col-lg-8 col-md-10 mx-auto">
                        <div class="site-heading">
                            <h1>{{ $page.general.edges[0].node.title }}</h1>
                            <span class="subheading">{{ $page.general.edges[0].node.subtitle }}</span>
                        </div>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Content -->
        <div class="container">
            <div class="row">
                <div class="col-lg-8 col-md-10 mx-auto">
                    <div
                        v-for="edge in $page.posts.edges"
                        :key="edge.node.id"
                        class="post-preview">
                        <g-link :to="'/post/' + edge.node.id">
                            <h2 class="post-title">
                                {{ edge.node.title }}
                            </h2>
                        </g-link>
                        <p class="post-meta">on {{ edge.node.created_at }}</p>
                        <p>
                            <g-link
                                v-for="(tag, index) in edge.node.tags"
                                :key="index"
                                :to="'/tag/' + tag.id">{{ tag.title }}  </g-link>
                        </p>
                        <hr>
                    </div>

                    <!-- Pager -->
                    <Pager :info="$page.posts.pageInfo"/>
                </div>
            </div>
        </div>


    </Layout>
</template>

<page-query>
query ($page: Int) {
    posts: allStrapiPost (perPage: 1, page: $page) @paginate {
        pageInfo {
            totalPages
            currentPage
        }
        edges {
            node {
                id,
                title,
                created_at,
                tags {
                    id,
                    title
                }
            }
        }
    }

    general: allStrapiGeneral {
        edges {
            node {
                id
                title
                subtitle
                cover {
                    url
                }
            }
        }
    }
}
</page-query>


<script>
    import { Pager } from 'gridsome'
    export default {
        name: 'HomePage',
        components: {
            Pager
        }
    }
</script>

