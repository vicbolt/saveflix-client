<template>
    <div class="sf-mi-ranking" v-if="visible">
        <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>
        <v-row>
            <v-col :cols="listaPeliculas">
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="background-color:rgb(229,9,20)">
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                    <v-card-title class="font-weight-black" style="font-size: 25px"> TOP PELICULAS </v-card-title>
                </v-app-bar>

                <v-card-text>
                        <div v-for="(movie, index) in ranking" :key="movie.id"  @click="goTo(movie._id)" id="background"  class="container-movies mb-5" :style="{backgroundImage:`url(${ movie.image })` }">
                            <v-row style="background-color: rgb(0,0,0,0.9); cursor: pointer" class="mr-0">
                                <v-col :cols="posicionM" class="numeros">
                                    <h1 class="text-center mt-6" style="color:white;">{{index + 1}}</h1>
                                </v-col>
                                <v-col :cols="imagenM">
                                    <v-img class="imagen" style="border-radius:100%" :src="movie.image" width="70px" height="70px" />
                                </v-col>
                                <v-col :cols="tituloM"  style="display: flex; justify-content: center" >
                                    <p class="mt-5 font-weight-black" style="font-size: auto; color:white;"> {{movie.title}} </p>
                                </v-col> 
                                <v-col :cols="puntuacionM" style="display: flex; justify-content: right" >
                                    <h4 class=" mt-2" :style="fontSize"> {{movie.score}} <br> <v-icon color="rgb(229,9,20)">mdi-heart</v-icon></h4> 
                                </v-col>
                                <div>
                                    <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                </v-card-text>         
            </v-card>
            </v-col>

            <v-col :cols="listaSeries">
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="font-size: 25px; background-color:rgb(229,9,20)" >
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                        <v-card-title class="font-weight-black" style="font-size: 25px"> TOP SERIES </v-card-title>
                </v-app-bar>
    
                <v-card-text>
                        <div v-for="serial, index in serialRanking" :key="serial.id" @click="goTo(serial._id)" id="background"  class="container-movies mb-5" :style="{backgroundImage:`url(${serial.image})`}">
                            <v-row style="background-color: rgb(0,0,0,0.9); cursor: pointer" class="mr-0">
                                <v-col :cols="posicionS" class="numeros" >
                                        <h1 class="text-center mt-6" style="color:white; text-align: center">{{index + 1}}</h1>
                                </v-col>
                                <v-col :cols="imagenS">
                                    <v-img class="imagen" style="border-radius:100%" :src="serial.image" width="70px" height="70px" />
                                </v-col>
                                <v-col :cols="tituloS"  style="display: flex; justify-content: center" >
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: auto; color:white;" > {{serial.title}} </p> 
                                </v-col> 
                                <v-col :cols="puntuacionS"  style="display: flex; justify-content: right"  >
                                    <h4 class="ml-10 mt-2" :style="fontSize"> {{serial.score}} <br> <v-icon color="rgb(229,9,20)">mdi-heart</v-icon></h4> 
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

            listaPeliculas: "",
            listaSeries: "",

            fontSize: "font-size:20px; text-align:center",

            posicionM : "1",
            imagenM: "3",
            tituloM: "5",
            puntuacionM: "3",

            posicionS : "1",
            imagenS: "3",
            tituloS: "5",
            puntuacionS: "3",

            value: this.$vuetify.breakpoint.name,


        }
    },

   created(){
        this.responsive()
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

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)
  
            if( value === "xl" || value === "lg" || value === "md"){
                this.listaPeliculas = "6"
                this.listaSeries = "6"
            }
            if(value === "sm") {
                this.listaPeliculas = "12"
                this.listaSeries = "12"
            }
            
            if ( value === "xs"){
                this.listaSeries = "12"
                this.listaPeliculas = "12"

                this.posicionM = "1"
                this.imagenM = "4"
                this.tituloM = "3"
                this.puntuacionM = "3"
                this.fontSize = "font-size:10px; text-align:center"
            
            }
        }
    },

    methods: {

        responsive(){

            if(this.value === "xl" || this.value === "lg" || this.value === "md"){
                this.listaPeliculas = "6"
                this.listaSeries = "6"
            }
            if(this.value === "sm") {
                this.listaPeliculas = "12"
                this.listaSeries = "12"
            }
            
            if (this.value === "xs"){
                this.listaSeries = "12"
                this.listaPeliculas = "12"

                this.posicionM = "1"
                this.imagenM = "4"
                this.tituloM = "3"
                this.puntuacionM = "3"
                this.fontSize = "font-size:10px; text-align:center"

                this.posicionS = "1"
                this.imagenS = "4"
                this.tituloS = "3"
                this.puntuacionS = "3"

                
            }

        },

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

.numeros{
    background-color: rgb(229, 9, 20);
    display: flex;
    align-content: center;
    justify-content: center;
}

</style>