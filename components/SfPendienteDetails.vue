<template>
    <div class="sf-pendiente-details">

        <v-alert v-if="this.error" class="text-center" dismissible type="error" border="top" color="red" dark> {{this.error}} </v-alert>
        <v-alert v-if="this.msg" type="info" class="text-center" dismissible border="top" color="green" dark> {{this.msg}} </v-alert>


        <v-card>
            <v-app-bar shaped style="background: white; color: black">
                    <v-icon class="mr-4" size="35" color="black"> mdi-star </v-icon>
                    <v-app-bar-title class="font-weight-black mt-1" style="font-size:22px" > {{post.title}} </v-app-bar-title>
                    <v-spacer width="20px"></v-spacer>
                    <v-btn @click="remove(post._id)"><v-icon color="rgb(229,9,20)">mdi-trash-can-outline</v-icon></v-btn>
            </v-app-bar>

            <v-card-text>
                <v-row>
                            <v-col cols="4" class="mt-3">
                                    <v-img height="420px" max-width="300px" style="border: 4px solid white" :src="post.image"></v-img>
                            </v-col>
                            <v-col cols="8" class="mt-3">
                                    <h6>DIRECTOR</h6>
                                    <v-divider class="mb-1" style="width: 160px;"></v-divider>
                                    <h3 class="mb-6"> {{post.director}}</h3>

                                    <v-btn v-if="post.post === 'movie'" @click="addPostP(post._id)" color="rgb(229,9,20)">AÑADIR A MI LISTA DE PELICULAS</v-btn>
                                    <v-btn v-if="post.post === 'serial'" @click="addPostP(post._id)" color="rgb(229,9,20)">AÑADIR A MI LISTA DE SERIES</v-btn>
                            </v-col>
                </v-row>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>

export default ({
    data(){
        return{
            userId: this.post.userId._id,
            admin: false,
            admin2: true,
            postUser: this.post.userId._id,
            postId: this.post._id,
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

        async addPostP(postId){
            try{
                
                this.$router.push(`/editPendiente/${postId}`)

                
            }catch(error){
                return console.log(error)
            }
        },

        async remove(){
            try{

            const config = require('../config')

            if(this.post.post === "movie"){
                const res = await fetch(config.hostname + `api/moviePendiente/remove/${this.post._id}`, {
                    method: "delete",
                    headers: {
                        'Content-Type' : 'application/json'
                    }
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }
                this.msg = "El post ha sido borrado"

                setTimeout(() => {
                    this.$router.push(`/miPerfil/${this.post.userId}`)
                }, 2000);

            } else {

                const res = await fetch(config.hostname + `api/serialPendiente/remove/${this.post._id}`, {
                    method: "delete",
                    headers: {
                        'Content-Type' : 'application/json'
                    }
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }
                this.msg = "El post ha sido borrado"
                setTimeout(() => {
                    this.$router.push(`/miPerfil/${this.post.userId}`)
                }, 2000);
            }

            } catch (error){
                return console.log(error)
            }
        },

    }
})

</script>

<style scoped>

.imagen{
    border: 3px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 1em;
}


</style>