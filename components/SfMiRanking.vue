<template>
    <div class="sf-mi-ranking">

        <v-row>
            <v-col cols=6>
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="background-color:rgb(229,9,20)">
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                    <v-card-title class="font-weight-black" style="font-size: 25px"> TOP PELICULAS </v-card-title>
                </v-app-bar>

                <v-card-text>
                    <v-list>
                        <div v-for="(movie, index) in ranking" :key="movie.id" @click="goTo(movie._id)" class="container-movies mb-5" style="background-color: black">
                            <v-row>
                                <v-col cols="1" style="background-color:rgb(229,9,20)">
                                    <h1 class="text-center mt-6" style="color:white;">{{index + 1}}</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" :src="movie.image" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="7">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px"> {{movie.title}} </p> 
                                </v-col> 
                                <div>
                                <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                    </v-list>
                </v-card-text>         
            </v-card>
            </v-col>

            <v-col cols=6>
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="font-size: 25px; background-color:rgb(229,9,20)" >
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                        <v-card-title class="font-weight-black"> TOP SERIES </v-card-title>
                </v-app-bar>
    
                <v-card-text>
                    <v-list>
                        <div v-for="serial, index in serialRanking" :key="serial.id" class="container-movies mb-5" style="background-color: black">
                            <v-row>
                                <v-col cols="1" style="background-color:rgb(229,9,20)">
                                        <h1 class="text-center mt-6" style="color:white;">{{index + 1}}</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" :src="serial.image" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="7">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px"> {{serial.title}} </p> 
                                </v-col> 
                                <div>
                                <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                    </v-list>
                </v-card-text>       

            </v-card>
            </v-col>
        </v-row>

    </div>
</template>

<script>

export default ({
    data(){
        return{
            ranking: [],
            serialRanking: [],
            moviePosition: 1,
            serialPosition: 1,
        }
    },

    async beforeMount(){
        await this.moviesRanking()
        await this.serialsRanking()

    },

    methods: {
        async moviesRanking(){
            try {

            const res = await fetch('http://localhost:4500/api/movie/getAll')

            const data = await res.json()
            if(data.error){
                return alert(data.error)
            }

            this.movies = []
            for(const movie of data.ranking){
                this.ranking.push(movie)
                this.moviePosition = this.moviePosition + 1
            }

            }catch(error){
                return res.json(error)
            }
        },

        async serialsRanking(){
            try {

            const res = await fetch('http://localhost:4500/api/serial/getAll')

            const data = await res.json()
            if(data.error){
                return alert(data.error)
            }

            this.serials = []

            for(const serial of data.serialRanking){
                this.serialRanking.push(serial)
            }

            }catch(error){
                return res.json(error)
            }
        },

        goTo(movieId){

            this.$router.push(`/details/${movieId}`)
        }
    }
})
</script>



<style scoped>

#fondo{
    background-color: white;
    color: black;
    padding: 3px;
    border-radius: 2%;
}
</style>