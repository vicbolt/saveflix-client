<template>
    <div class="sf-moviesP">

        <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-row>
                    <v-col cols="9">
                        <v-app-bar-title class="font-weight-black mt-2" style="font-size: 15px"> <v-icon > mdi-clock</v-icon> PELICULAS PENDIENTES </v-app-bar-title>
                    </v-col>
                    <v-col cols="3">
                        <v-btn color="rgb(229,9,20)" style="color:white" @click="goTo()"> AÑADIR</v-btn>
                    </v-col>
                </v-row>
            </v-app-bar>

                <v-card-text>
                    <v-row>
                        <v-col cols="3" v-for="movie in movies" :key="movie._id" class="mb-7">
                                <v-list-item-content style="cursor: pointer" class="mb-n12" @click="go(movie._id)">
                                    <v-img height="130px" style="border: 2px solid white" :src="movie.image" />
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{movie.title}} </v-list-item-title>
                                </v-list-item-content>
                        </v-col>
                    </v-row>
                </v-card-text>

        </v-card>
    </div>
</template>

<script>

export default({
    data(){
        return{
            movies: []
        }
    },

    async beforeMount(){
        await this.loadMoviesPendientes()
    },

    methods: {

        async loadMoviesPendientes(){
            try{

                const userId = JSON.parse(localStorage.getItem("userId"))

                const config = require('../config')

                const res = await fetch(config.hostname + `api/moviePendiente/getAll/${userId}`)

                const data = await res.json()
                if(data.error){
                    return console.log(data.error)
                }

                const movieP = data.moviesPendientes

                for(const movie of movieP){
                    this.movies.push(movie)
                }



            }catch(error){
                return console.log(error)
            }
        },

        goTo(){
            this.$router.push('/addMovieP')
        },

        go(id){
            this.$router.push(`/pendienteDetails/${id}`)
        }
    }

})
</script>
