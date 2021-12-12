<template>
    <div class="sf-add-movie">

        <v-card>
            <v-app-bar>
                <v-icon class="font-weight-black mr-2" size="40"> mdi-plus </v-icon>
                <v-app-bar-title class="font-weight-black"> AÑADIR SERIE </v-app-bar-title>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-col cols="4">
                        <v-file-input v-model="image" class="mt-3" height="360px" outlined prepend-icon="" style="background: gray; border: 2px solid white" placeholder="AÑADIR PORTADA"/>
                    </v-col>

                    <v-col cols="8">
                        <h6>TITULO DE LA PELICULA</h6>
                        <v-divider class="mb-1" ></v-divider>
                        <v-text-field v-model="title" placeholder="Ej: 'La Casa de Papel'" outlined />
                        <h6 class="mt-n5">DIRECTOR</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="director" placeholder="Ej: 'Rubén Molina'" outlined />
                        <h6 class="mt-n5">SINOPSIS</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="description" placeholder="Escriba su opinión sobre la serie" outlined height="100px" />
                        <h6 class="mt-n5">PUNTUACIÓN</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-row>
                            <v-col cols="2">
                                <v-text-field v-model="score" :onkeydown="showScore()" :rules="scoreRules" height="30px" placeholder="0/100" outlined ></v-text-field>
                            </v-col>
                            <v-col cols="10">
                                <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="55">
                                    <strong> {{ Math.ceil(knowledge) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                </v-progress-linear>
                            </v-col>
                        </v-row>
                    </v-col>

                    <v-btn color="red" @click="upload" block> SUBIR SERIE </v-btn>
                </v-row>
            </v-card-text>
        </v-card>


    </div>
</template>

<script>
export default ({
    data() {
        return{
            title: "",
            director: "",
            description: "",
            score: "",
            image: undefined,
            knowledge: "0",
            scoreRules: [
                v => ( v && v >= 0 ) || "El valor mínimo es 0",
                v => ( v && v <= 100 ) || "El valor máximo es 100",
            ],
        }
    },

    methods: {
        async upload(){

            const strUserId = localStorage.getItem('userId')

            const userId = JSON.parse(strUserId)
            
            const formData = new FormData()
                formData.enctype = 'multipart/form-data'
                formData.append('title', this.title)
                formData.append('director', this.director)
                formData.append('description', this.description)
                formData.append('score', this.score)
                formData.append('userId', userId)
                formData.append('image', this.image)
            
            try{

                const config = require('../config')

                const res = await fetch(config.hostname + 'api/serial/create', {
                    method: 'post',
                    body: formData
                })

                const data = await res.json()
                if(data.error){
                    return alert(data.error)
                }

                console.log(data.serial)

                alert('El post se ha subido con éxito')
                this.$router.push('/misSeries')

            }catch(error){
                return res.json(error)
            }
        },

        showScore(){
            this.knowledge = this.score
        },
    }
})
</script>
