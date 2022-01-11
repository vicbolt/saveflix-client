<template>
    <div class="sf-mi-ranking" v-if="visible">

        <v-alert v-if="this.error" type="error" border="top" color="red" dark> {{this.error}} </v-alert>


        <v-row>
            <v-col cols=6>
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="background-color:rgb(229,9,20)">
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                    <v-card-title class="font-weight-black" style="font-size: 25px"> TOP PELICULAS </v-card-title>
                </v-app-bar>

                <v-card-text>
                        <div v-for="(movie, index) in ranking" :key="movie.id"  @click="goTo(movie._id)" id="background"  class="container-movies mb-5" :style="{backgroundImage:`url(${ movie.image })` }">
                            <v-row style="background-color: rgb(0,0,0,0.9); cursor: pointer" class="mr-0">
                                <v-col cols="1" style="background-color:rgb(229,9,20)">
                                    <h1 class="text-center mt-6" style="color:white;">{{index + 1}}</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" :src="movie.image" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="5">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px; color:white;"> {{movie.title}} </p>
                                </v-col> 
                                <v-col cols="3" >
                                    <h4 class="ml-10 mt-2" style="font-size:20px; text-align:center"> {{movie.score}} <br> <v-icon color="rgb(229,9,20)">mdi-heart</v-icon></h4> 
                                </v-col>
                                <div>
                                    <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                </v-card-text>         
            </v-card>
            </v-col>

            <v-col cols=6>
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="font-size: 25px; background-color:rgb(229,9,20)" >
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                        <v-card-title class="font-weight-black" style="font-size: 25px"> TOP SERIES </v-card-title>
                </v-app-bar>
    
                <v-card-text>
                        <div v-for="serial, index in serialRanking" :key="serial.id" @click="goTo(serial._id)" id="background"  class="container-movies mb-5" :style="{backgroundImage:`url(${serial.image})`}">
                            <v-row style="background-color: rgb(0,0,0,0.9); cursor: pointer" class="mr-0">
                                <v-col cols="1" style="background-color:rgb(229,9,20)" >
                                        <h1  class="text-center mt-6" style="color:white;">{{index + 1}}</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" :src="serial.image" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="5">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px; color:white;"> {{serial.title}} </p> 
                                </v-col> 
                                <v-col cols="3" >
                                    <h4 class="ml-10 mt-2" style="font-size:20px; text-align:center"> {{serial.score}} <br> <v-icon color="rgb(229,9,20)">mdi-heart</v-icon></h4> 
                                </v-col>
                                <div>
                                <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                </v-card-text>       

            </v-card>
            </v-col>
        </v-row>

    </div>
</template>

<script>
export default ({
    layout: 'ranking',

    data(){
        return{
            visible: true,
            ranking: [],
            serialRanking: [],
            moviePosition: 1,
            serialPosition: 1,
            error: "",
        }
    },

    async beforeMount(){
        const token = localStorage.getItem("token")
            if(!token){
                this.visible = false
                return this.$router.push("/logIn")
            }
            
        await this.moviesRanking()
        await this.serialsRanking()

    },

    methods: {
        async moviesRanking(){
            try {

                const strId = localStorage.getItem('userId')
                const id = JSON.parse(strId)

            const config = require('/config')

            const res = await fetch(config.hostname + `api/movie/getAll/${id}`)

            const data = await res.json()
            if(data.error){
                return this.error = data.error
            }

            this.movies = []
            for(const movie of data.ranking){
                this.ranking.push(movie)
                this.moviePosition = this.moviePosition + 1
            }

            }catch(error){
                return console.log(error)
            }
        },

        async serialsRanking(){
            try {

                const config = require('/config')

                const strId = localStorage.getItem('userId')
                const id = JSON.parse(strId)

                const res = await fetch(config.hostname + `api/serial/getAll/${id}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                this.serials = []

                for(const serial of data.serialRanking){
                    this.serialRanking.push(serial)
                }

            }catch(error){
                return console.log(error)
            }
        },

        goTo(postId){
            localStorage.setItem("postId", postId)
            this.$router.push(`/details/${postId}`)
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

#background{
    background-size: 100% ;
}


#background:hover{
    zoom: 130%;
}

</style>