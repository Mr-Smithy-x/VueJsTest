<template>
    <div class="listfeed">


        <div class="container">
            <form @submit.prevent="searchArticle">
                <input class="media-input" placeholder="Enter Article" v-model="query" v-validate="'min:5'"
                       name="query"/>


                <transition name="alert-in" enter-active-class="animated flipInX"
                            leave-active-class="animated flipOutX">
                    <p class="alert" v-if="errors.has('query')">{{errors.first('query')}}</p>
                </transition>
            </form>

            <ul class="media-list">

                <transition-group name="list" enter-active-class="animated bounceInUp"
                                  leave-active-class="animated bounceOutDown">

                    <li class="media" v-for="(article, index) in articles[page-1]" :key="index">
                        <div class="media-left">
                            <a v-bind:href="article.url" target="_blank">
                                <img class="media-object" v-bind:src="article.urlToImage"/>
                            </a>
                        </div>
                        <div class="media-body">
                            <h4 class="media-heading">
                                <a v-bind:href="article.url" target="_blank">
                                    {{article.title}}
                                </a>
                            </h4>

                            <h5>
                                <i>by {{article.author}}</i>
                            </h5>

                            <p class="media-desc">{{article.description}}</p>
                        </div>


                        <router-link :to="{ name: 'article',params: {id: article.id}}"><i
                                class="fa fa-edit"></i></router-link>
                    </li>
                </transition-group>
            </ul>
        </div>


        <Pagination v-if="query === undefined || query.length === 0" :total="total" :current="page" :limit="20"
                    :pages="pages" :query="query" :name-value="'list_page'"/>
        <Pagination v-else-if="query.length > 0" :total="total" :current="page" :limit="20" :pages="pages"
                    :query="query" :name-value="'list_query_page'"/>

    </div>

</template>

<script>
    /* eslint-disable no-console */

    import Pagination from "../pagination/Pagination";

    export default {
        name: "ListFeed",
        components: {Pagination},
        props: ['source'],
        data() {
            return {
                articles: [],
                query: this.$route.params.query,
                page: Math.floor(this.$route.params.page) > 0 ? Math.floor(this.$route.params.page) : 1,
                total: 0,
                pages: 0
            }
        },
        methods: {

            searchArticle() {
                this.$validator.validateAll().then((result) => {

                    if (result) {
                        this.page = 1;
                        this.updateSource(this.query)
                    } else {
                        console.log("Search Query to short")
                    }
                });
            },
            updateSource: function (query = "") {
                this.articles.length = 0;
                if (query.length > 0) {
                    this.$http.get('https://www.yoprice.co/api/news/news?q=' + query + '&language=en&sources=cnn&page=' + this.page.toString())
                        .then(response => {
                            console.log(response.data.pagination);
                            console.log(response.data);

                            this.total = response.data.pagination.total;
                            this.pages = response.data.pagination.pages;
                            this.articles[this.page - 1] = (response.data.articles);
                        })
                } else {
                    this.$http.get('https://www.yoprice.co/api/news/news?sources=cnn&language=en&page=' + this.page.toString())
                        .then(response => {

                            this.total = response.data.pagination.total;
                            this.pages = response.data.pagination.pages;
                            this.articles[this.page - 1] = (response.data.articles);
                        })
                }
            }
        },
        created: function () {
            this.updateSource(this.$route.params.query);
        },
        watch: {
            query: function (query) {
                if (query.length === 0) {
                    this.page = 1;
                    this.updateSource(query)
                }
            },
            source: function (val) {
                this.updateSource(val);
                this.page++;
            },
            page: function (page) {
                if (page > 0) {
                    this.page = 1;
                    this.updateSource(this.query)
                }
            }
        },


    }
</script>

<style src="./feed.css" scoped>

</style>