<template>
    <div>

        <genre-component></genre-component>

        <v-row v-scroll="onScroll">
            <v-col cols="12" lg="3" md="6" sm="6" v-for="movies in resultsTrending" :key="movies.id">
                <v-skeleton-loader class="mx-auto" type="image, card-heading, list-item, list-item-three-line"
                    v-if="dataFetching == true">
                </v-skeleton-loader>

                <v-hover v-else>
                    <template v-slot:default="{ hover }">

                        <v-card >
                            <v-img :src="imagePath+movies.poster_path" height="250"></v-img>

                            <v-card-title>
                                <span class="font-weight-bold text-no-wrap"> {{movies.title}} </span>
                            </v-card-title>
                            <v-card-subtitle>
                                <span class="primary--text">
                                    Release Date: {{movies.release_date}}
                                </span>
                            </v-card-subtitle>

                            <v-card-text>
                                <div class="d-flex align-items-center">
                                    <v-rating :value="movies.vote_average" color="amber" length="10" dense
                                        half-increments readonly size="14"></v-rating>

                                    <div class="grey--text ms-4">
                                        {{movies.vote_average}} ({{movies.vote_count}})
                                    </div>
                                </div>
                                
                            </v-card-text>

                            <!-- <v-divider class="mx-4"></v-divider>
                            <v-card-title>Genres</v-card-title>

                            <v-card-text>
                                <v-chip-group column>
                                    {{movies.genre_ids}}
                                    <v-chip>5:30PM</v-chip>

                                </v-chip-group>
                            </v-card-text> -->

                            <v-fade-transition>
                                <v-overlay v-if="hover" absolute color="#036358">
                                    <div class="d-flex flex-column justify-content-center align-items-center">
                                        <v-btn @click="showDetails(movies)">View Details</v-btn>
                                        or,
                                        <v-btn small color="indigo" class="mt-3 white--text" @click="addWatchList(movies)" >
                                            <v-icon left>mdi-star</v-icon> Add to Watchlist
                                        </v-btn>
                                    </div>
                                </v-overlay>
                            </v-fade-transition>

                        </v-card>
                    </template>
                </v-hover>

            </v-col>

        </v-row>

    </div>
</template>

<script>
    import genre from "../genre.vue"
    export default {
        components: {
            "genre-component": genre,
        },
        data() {
            return {
                results: [],
                //checkID: '',
            }
        },

        methods: {

            onScroll() {

                if (window.scrollY + window.innerHeight >= document.body.scrollHeight - 50) {
                    this.page += 1;
                    this.getMovieTrending();

                }


            },

            showDetails(data) {
                this.$router.push({
                    name: 'Details',
                    params: {
                        details: data
                    }
                })
            }
        },

        created() {
            this.getMovieTrending();

        },

        watch: {
            searchData: function (e) {

                if (e) {
                    this.resultsTrending = e;
                }

            },

            genreData: function (e) {

                if (e) {
                    this.resultsTrending = e;
                }

            }
        }

    }

</script>
