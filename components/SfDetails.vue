<template>
    <div class="sf-details">

        <v-card>
            <v-app-bar shaped style="background: white; color: black">
                    <v-icon class="mr-4" size="35" color="black"> mdi-star </v-icon>
                    <v-app-bar-title class="font-weight-black mt-1" style="font-size:22px" > {{post.title}} </v-app-bar-title>
                    <v-spacer width="20px"></v-spacer>
                    <v-btn v-if="admin" @click="edit(post._id)" class="mr-2"><v-icon color="green">mdi-movie-edit-outline</v-icon></v-btn>
                    <v-btn v-if="admin" @click="remove(post._id)"><v-icon color="red">mdi-trash-can-outline</v-icon></v-btn>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3">
                                    <v-img height="420px" max-width="300px" style="border: 4px solid white" :src="post.image"></v-img>
                                    <v-btn @click="goTo(post.user._id)">{{post.user.username}}</v-btn>
                            </v-col>
                            <v-col cols="8">
                                    <h6 class="mb-1">DIRECTOR</h6>
                                    <v-divider class="mb-1" style="width: 160px;"></v-divider>
                                        <h3 class="mb-6"> {{post.director}}</h3>

                                    <h6 class="mb-1">SINOPSIS</h6>
                                    <v-divider class="mb-2" style="width: 560px;"></v-divider>
                                        <p style="max-width:560px; height:180px; text-align: justify; overflow: scroll" class="mb-10"> {{post.description}}</p>
                                    
                                    <h6 class="mb-1 mt-8">PUNTUACIÓN</h6>
                                    <v-divider class="mb-1" style="width: 560px;"></v-divider>
                                    <v-progress-linear color="rgb(229,9,20)" v-model="post.score" height="25" style="width: 560px;">
                                            <strong> {{ Math.ceil(post.score) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                    </v-progress-linear>

                                    <v-row class="mt-4" style="width: 740px;">
                                        <v-col cols="3" >
                                            <v-row >
                                                <v-btn class="ml-2" @click="like(post._id)">
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-heart </v-icon>
                                                    <p class="ml-n3 mt-4">{{post.likes}}</p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-row>
                                                <v-btn >
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-eye </v-icon>
                                                    <p class="ml-n3 mt-4">{{post.views}}</p>
                                                </v-btn>
                                            </v-row>

                                        </v-col>
                                        <v-col cols="1">
                                            
                                        </v-col>
                                        <v-col cols="3">
                                            <v-row>
                                                <v-btn v-if="admin2"  class="ml-7"  @click="addPost()">
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-share-variant-outline </v-icon>
                                                    <p class="ml-n3 mt-4" style="font-size: 11px" > AÑADIR A MI LISTA </p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                    </v-row>
                            </v-col>
                        </v-list-item-content>
                    </v-list-item>
                </v-row>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>

export default ({
    data(){
        return{
            userId: this.post.user._id,
            admin: false,
            admin2: true,
            postUser: this.post.user._id,
            postId: this.post._id
            
        }
    },
    props: {
        post: {
            type: Object,
            required: true
        }
    },

    async beforeMount(){
        try{


        //ADMIN? (MUESTRA U OCULTA LOS BOTONES DE EDITAR Y ELIMINAR DEPENDIENDO SI ES UNA PUBLICACION TUYA O NO)

        const strUserId = localStorage.getItem('userId')

        const userId = JSON.parse(strUserId)

        const postUser = this.postUser

        if(userId === postUser){
            this.admin = true
            this.admin2 = false
        }


        // VIEWS
        const config = require('../config')
        const postId = this.postId

        const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
        const data = await res.json()

            if(data.movie !== null){
                const body = JSON.stringify({
                    postId: this.postId
                })

                const res = await fetch(config.hostname + 'api/movie/views',{
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    method: 'post',
                    body,
                })

                const data = await res.json()
                if(data.error){
                    return res.json(data.error)
                }

            } else {

                const body = JSON.stringify({
                postId: this.postId
                })

                const res = await fetch(config.hostname + 'api/serial/views',{
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    method: 'post',
                    body,
                })

                const data = await res.json()
                if(data.error){
                    return res.json(data.error)
                }

            }


            }catch(error){
                return console.log(error)
            }

    },


    methods:{
        async remove(postId){
            try{

                const config = require('../config')

                const postId = this.postId

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){
                    const res = await fetch(config.hostname + `api/movie/remove/${postId}`,{
                    method: 'delete',
                    headers: {
                        'Content-type':"application/json"
                    }
                    })

                    const data = await res.json()
                    if(data.error){
                        return alert(data.error)
                    }

                    alert('El post ha sido borrado con éxito')
                    this.$router.push('/misPeliculas')

                } else {

                    const res = await fetch(config.hostname + `api/serial/remove/${postId}`,{
                    method: 'delete',
                    headers: {
                        'Content-type':"application/json"
                    }
                    })

                    const data = await res.json()
                    if(data.error){
                        return alert(data.error)
                    }

                    alert('El post ha sido borrado con éxito')
                    this.$router.push('/misSeries')
                }

            }catch(error){
                return res.json(error)
            }
        },

        async like(postId){
            try{
                const config = require('../config')

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){

                    const body = JSON.stringify({postId})

                    const res = await fetch(config.hostname + 'api/movie/likes',{
                        method: 'post',
                        headers: {
                            'Content-Type' : 'application/json',
                        },
                        body
                    })

                    const data = await res.json()
                    if(data.error){
                        return alert(data.error)
                    }
                    
                this.$router.go(`/details/${postId}`)

                } else {

                    const body = JSON.stringify({postId})

                    const res = await fetch(config.hostname + 'api/serial/likes',{
                        method: 'post',
                        headers: {
                            'Content-Type' : 'application/json',
                        },
                        body
                    })

                    const data = await res.json()
                    if(data.error){
                        return alert(data.error)
                    }

                    this.$router.go(`/details/${postId}`)
            }

            }catch(error){
                return res.json(error)
            }
        },

        // async addPost(){
        //     try{

        //         if(this.post.post === "movie"){
                
        //             const strUserId = localStorage.getItem('userId')

        //             const userId = JSON.parse(strUserId)
                    
        //             const formData = new FormData()
        //                 formData.enctype = 'multipart/form-data'
        //                 formData.append('title', this.post.description)
        //                 formData.append('director', this.post.director)
        //                 formData.append('description', this.post.description)
        //                 formData.append('score', this.post.score)
        //                 formData.append('userId', userId)
        //                 formData.append('image', this.post.image)


        //                 console.log(this.post.title)
        //                 console.log(this.post.director)
        //                 console.log(this.post.description)
        //                 console.log(this.post.score)
        //                 console.log(userId)
        //                 console.log(this.post.image)
        //                 console.log(this.post.post)

        //             const res = await fetch('http://localhost:4500/api/movie/duplicate', {
        //                 method: 'post',
        //                 body: formData
        //             })

        //             const data = await res.json()
        //             if(data.error){
        //                 return alert(data.error)
        //             }

        //             console.log(data.movie)

        //             alert('El post se ha guardado con éxito')
        //             this.$router.push(`/misPeliculas/${userId}`)
        //             }

        //     }catch(error){
        //         return res.json(error)
        //     }
        // },


        edit(postId){

            this.$router.push(`/edit/${postId}`)
        },

        goTo(ownerId){
            
            localStorage.setItem("ownerId", ownerId)
            
            const userId = JSON.parse(localStorage.getItem("userId"))

            if(ownerId === userId){
                return this.$router.push(`/miPerfil/${userId}`)
            }

            this.$router.push(`/perfil/${ownerId}`)
        }
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