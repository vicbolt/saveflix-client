<template>
    <div class="sf-add-movieP">

        <v-alert v-if="this.error" border="top" type="error" color="red" dark> {{this.error}} </v-alert>

        <v-card>
            <v-app-bar>
                <v-icon class="font-weight-black mr-2" size="40"> mdi-plus </v-icon>
                <v-app-bar-title class="font-weight-black"> AÑADIR PELICULA </v-app-bar-title>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-col cols="4">
                        <v-file-input v-model="image" class="mt-3" height="360px" filled prepend-icon="" style="background: gray; border: 2px solid white" placeholder="AÑADIR PORTADA"/>
                    </v-col>

                    <v-col cols="8">
                        <h6>TITULO DE LA PELICULA</h6>
                        <v-divider class="mb-1" ></v-divider>
                        <v-text-field v-model="title" name="title" placeholder="Ej: 'El resplandor'" outlined />

                        <h6 class="mt-n5">DIRECTOR</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="director" name="director" placeholder="Ej: 'Stanley Kubrick'" outlined />

                    </v-col>

                    <v-btn color="rgb(229,9,20)" @click="upload" block> SUBIR A PELICULAS PENDIENTES </v-btn>
                </v-row>
            </v-card-text>
        </v-card>


    </div>
</template>

<script>
export default {
    data() {
        return{
            title: "",
            director: "",
            image: undefined,
            error: "",
        }
    },

    methods: {
        async upload(){

            try{

                const config = require('../config')

                const strUserId = localStorage.getItem('userId')

                const userId = JSON.parse(strUserId)
                
                const formData = new FormData()
                    formData.enctype = 'multipart/form-data'
                    formData.append('title', this.title)
                    formData.append('director', this.director)
                    formData.append('userId', userId)
                    formData.append('image', this.image)

                const res = await fetch(config.hostname + 'api/moviePendiente/create', {
                    method: 'post',
                    body: formData
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }


                alert('El post se ha subido con éxito')
                this.$router.push('/misPeliculas')

            }catch(error){
                return console.log(error)
            }
        },


    }

}
</script>
