<template>
    <div class="sf-edit-pendiente">

        <v-card>
            <v-app-bar>
                <v-icon class="font-weight-black mr-2" size="40"> mdi-movie-edit-outline </v-icon>
                <v-app-bar-title class="font-weight-black"> EDITAR </v-app-bar-title>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-col cols="4">
                        <v-img v-model="image" :src="image" class="mt-3" height="400px"/>
                    </v-col>

                    <v-col cols="8">
                        <h6>TITULO DE LA PELICULA</h6>
                        <v-divider class="mb-1" ></v-divider>
                        <v-text-field v-model="title" :placeholder="post.title" outlined />

                        <h6 class="mt-n5">DIRECTOR</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="director" :placeholder="post.director" outlined /> 

                        <h6 class="mt-n5">SINOPSIS</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="description" :placeholder="post.description" outlined height="100px" />

                        <h6 class="mt-n5">PUNTUACIÓN</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-row>
                            <v-col cols="2">
                                <v-text-field v-model="score" :onkeydown="showScore()" :rules="scoreRules" height="30px" outlined ></v-text-field>
                            </v-col>
                            <v-col cols="10">
                                <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="55">
                                    <strong> {{ Math.ceil(knowledge) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                </v-progress-linear>
                            </v-col>
                        </v-row>
                    </v-col>

                    <v-btn color="rgb(229,9,20)" @click="addPostP()" block> GUARDAR </v-btn>
                </v-row>
            </v-card-text>
        </v-card>

    </div>
</template>

<script>

export default ({
    data(){
        return{
            title: this.post.title,
            director: this.post.director,
            description: this.post.description,
            image: this.post.image,
            score: this.post.score,
            knowledge: "0",
            scoreRules: [
                v => ( v && v >= 0 ) || "El valor mínimo es 0",
                v => ( v && v <= 100 ) || "El valor máximo es 100",
            ],
        }
    },

    props: {
        post: {
            type: Object,
            required: true
        }
    },


    methods: {

        showScore(){
            this.knowledge = this.score
        },

        async addPostP(){
            try{

                if(this.post.post === "movie"){

                    const userId = JSON.parse(localStorage.getItem("userId"))

                    const body = JSON.stringify({
                        title: this.title,
                        director: this.director,
                        description: this.description,
                        score: this.score,
                        userId: userId,
                        image: this.image
                    })

                    const config = require('../config')

                    const res = await fetch(config.hostname + 'api/movie/duplicate', {
                        method: "post",
                        headers: {
                            'Content-Type' : "application/json"
                        },
                        body
                    })

                    const data = await res.json()
                    if(data.error){
                        return console.log(data.error)
                    }

                    const res1 = await fetch(config.hostname +  `api/moviePendiente/remove/${this.post._id}`, {
                        method: "delete",
                        headers: {
                            'Content-Type': "application/json"
                        }
                    })

                    const data1 = await res1.json()
                    if(data1.error){
                        return console.log(data1.error)
                    }
                    
                    alert('El post se ha añadido a tus películas vistas')
                    return this.$router.push(`/misPeliculas/${userId}`)

                } else {

                    const userId = JSON.parse(localStorage.getItem("userId"))

                    const body = JSON.stringify({
                        title: this.title,
                        director: this.director,
                        description: this.description,
                        score: this.score,
                        userId: userId,
                        image: this.image
                    })

                    const config = require('../config')

                    const res = await fetch(config.hostname + 'api/serial/duplicate', {
                        method: "post",
                        headers: {
                            'Content-Type' : "application/json"
                        },
                        body
                    })

                    const data = await res.json()
                    if(data.error){
                        return console.log(data.error)
                    }

                    const res1 = await fetch(config.hostname +  `api/serialPendiente/remove/${this.post._id}`, {
                        method: "delete",
                        headers: {
                            'Content-Type': "application/json"
                        }
                    })

                    const data1 = await res1.json()
                    if(data1.error){
                        return console.log(data1.error)
                    }
                    
                    alert('El post se ha añadido a tus series vistas')
                    return this.$router.push(`/misSeries/${userId}`)
                }

            }catch(error){
                return console.log(error)
            }
        },
        

    }


})
</script>
