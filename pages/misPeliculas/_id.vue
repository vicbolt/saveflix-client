<template>
    <div class="sf-mis-peliculas" v-if="visible">

    <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>


        <v-card elevation="2">
            <v-app-bar shaped>
                <v-icon class="mr-4" size="35"> mdi-movie-open-star-outline </v-icon>
                <v-app-bar-title class="font-weight-black mt-1" style="font-size:20px" > MIS PELICULAS </v-app-bar-title>
                <v-spacer></v-spacer>
                <v-btn color="rgb(229,9,20)" @click="addMovie"> AÑADIR PELICULA </v-btn>
            </v-app-bar>
            <v-card-text>
                <h1 v-if="noMovies === true"> NO TIENES PELÍCULAS AÚN</h1>
                <div v-if="loading" style="text-align:center">
                    <img height="120em" src="@/static/load.gif" class="mb-6" />  
                </div>
                <v-row v-if="noMovies === false">
                    <v-col :cols="peliculas"  v-for="movie in movies" :key="movie.id" @click="goTo(movie._id)">
                        <v-list-item >
                            <v-list-item-content style="cursor: pointer;" class="fondo">
                                <v-img :height="size" :src="movie.image"/>
                                <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:20px"> {{movie.title}} </v-list-item-title>
                                        <v-progress-linear color="rgb(229,9,20)" v-model="movie.score" height="25">
                                            <strong> {{ Math.ceil(movie.score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                    </v-progress-linear>
                                <v-btn class="boton">ver mas...</v-btn>
                            </v-list-item-content>
                        </v-list-item>
                    </v-col>
                </v-row>
            </v-card-text>
        </v-card>
    </div>
</template>


<script>
export default ({
    layout: 'movieP',

    data() {
        return{
            visible: true,
            movies: [],
            noMovies: "",
            error: "",
            peliculas: "4",
            size: "360px",
            loading: false
        }
    },

    async beforeMount(){

        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")
        }

        this.loading = true

        await this.loadMovies()

        this.loading = false
        
    },

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)

            if( value === "xl" || value === "lg"){
                this.peliculas = "4"
                this.size = "360px"

            } else if(value === "lg" || value === "md"){
                this.peliculas = "6"
                this.size = "820px"
                
            } else if(value === "sm" || value === "xs") {
                this.peliculas = "12"
                this.size= "960px"

            }
        }
    },

    methods: {
        addMovie(){
            this.$router.push('/addMovie')
        },

        async loadMovies(){
            try{

                const strParamsId = localStorage.getItem('userId')

                const paramsId = JSON.parse(strParamsId)

                const config = require('/config')

                const res = await fetch(config.hostname + `api/movie/getAll/${paramsId}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }
                

                this.movies = []
                for(const movie of data.movies){
                    this.movies.push(movie)
                }

                if(this.movies.length === 0){
                    this.noMovies = true
                } else {
                    this.noMovies = false
                }



            }catch(error){
                return console.log(error)
            }
        },

        goTo(movieId){
            localStorage.setItem("postId", movieId)
            this.$router.push(`/details/${movieId}`)
        }
    }
})
</script>

<style scoped>

.fondo{
    margin-top: 1em;
    background-color: white;
    color: black;
    padding: 2px;
}

.boton{
    background-color: rgb(18, 18, 18);
    border: 2px solid rgb(229,9,20);
}
</style>