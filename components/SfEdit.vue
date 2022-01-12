<template>
    <div class="sf-edit">

    <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>
    <v-alert v-if="this.msg" type="info" class="text-center" dismissible border="top" color="green" dark> {{this.msg}} </v-alert>
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

                        <h6 class="mt-n5">OPINION</h6>
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

                    <v-btn color="rgb(229,9,20)" @click="update()" block> GUARDAR </v-btn>
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
            error: "",
            msg: "",
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


        async update(){
            try{
                const config = require('../config')

                const userId = JSON.parse(localStorage.getItem("userId"))

                console.log(userId)

                const body = JSON.stringify({
                    title: this.title,
                    description: this.description,
                    director: this.director,
                    image: this.image,
                    score: this.score,
                    userId: userId
                })

                const id = this.post._id

                if(this.post.post === "movie"){
                    
                    const res = await fetch(config.hostname + `api/movie/update/${id}`, {
                        method: 'put',
                        headers: {
                            'Content-type': 'application/json'
                        },
                        body,
                    })

                    const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    this.msg = 'El post se ha guardado con éxito'
                    setTimeout(() => {
                        this.$router.push(`/misPeliculas/${this.userId}`)
                    }, 2000);

                } else {

                    const res = await fetch(config.hostname + `api/serial/update/${id}`, {
                        method: 'put',
                        headers: {
                            'Content-type': 'application/json'
                        },
                        body,
                    })

                    const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    this.msg = 'El post se ha guardado con éxito'
                    setTimeout(() => {
                        this.$router.push(`/misSeries/${this.userId}`)
                    }, 2000);
                }

            }catch(error){
                return console.log(error)
            }
        }

    }


})
</script>
