<template>
    <div class="sf-mis-peliculas" v-if="visible">

        <v-card elevation="2">
            <v-app-bar shaped>
                <v-icon class="mr-4" size="35"> mdi-movie-open-star-outline </v-icon>
                <v-app-bar-title class="font-weight-black mt-1" style="font-size:20px" > MIS PELICULAS </v-app-bar-title>
                <v-spacer></v-spacer>
                <v-btn color="rgb(229,9,20)" @click="addMovie"> AÑADIR PELICULA </v-btn>
            </v-app-bar>
            <v-card-text>
                <h1 v-if="noMovies === true"> NO TIENES PELÍCULAS AÚN</h1>
                <v-row v-if="noMovies === false">
                    <v-col cols=4  v-for="movie in movies" :key="movie.id" @click="goTo(movie._id)">
                        <v-list-item>
                            <v-list-item-content style="cursor: pointer" class="fondo">
                                <v-img height="280px" :src="movie.image"/>
                                <v-list-item-title class="text-center mt-2 font-weight-black"> {{movie.title}} </v-list-item-title>
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
        }
    },

    async beforeMount(){

        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")
        }

        await this.loadMovies()
        
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
                    return alert(data.error) 
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