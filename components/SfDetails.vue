<template>
    <div class="sf-details">

        <v-card>
            <v-app-bar shaped style="background: white; color: black">
                <v-row>
                    <v-col cols="9">
                        <h2 class="font-weight-black mt-1" style="font-size:22px" ><v-icon class="mr-4 mb-1" size="35" color="black"> mdi-star </v-icon> {{post.title}} </h2>
                    </v-col>
                    
                    <v-col cols="3">
                    <v-btn v-if="admin"  @click="edit(post._id)" class="mr-2"><v-icon color="green">mdi-movie-edit-outline</v-icon></v-btn>
                    <v-btn v-if="admin" @click="remove(post._id)"><v-icon color="rgb(229,9,20)">mdi-trash-can-outline</v-icon></v-btn>
                    </v-col>
                </v-row>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3">
                                    <v-img height="420px" max-width="300px" style="border: 4px solid white" :src="post.image"></v-img>
                                    <v-btn @click="goTo(post.userId._id)">{{post.userId.username}}</v-btn>
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
                                                <v-btn class="ml-2" @click="like(post._id, post.userId._id)">
                                                    <v-icon v-if="this.liked" class="mr-4" size="20" color="red"> mdi-heart </v-icon>
                                                    <v-icon v-if="!this.liked" class="mr-4" size="20" color="white"> mdi-heart </v-icon>
                                                    <p class="ml-n3 mt-4">{{post.likes.length}}</p>
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
                                                <v-btn v-if="admin2"  class="ml-n12"  @click="addPost()">
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
            admin: false,
            admin2: true,
            ownerId: "",
            liked: "",
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

        //LIKED OR NOT
        await this.likedOrNot()

        //ADMIN? (MUESTRA U OCULTA LOS BOTONES DE EDITAR Y ELIMINAR DEPENDIENDO SI ES UNA PUBLICACION TUYA O NO)

        const strUserId = localStorage.getItem('userId')

        const userId = JSON.parse(strUserId)

        const postUser = this.post.userId._id

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
                    return console.log(data.error)
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
                    return console.log(data.error)
                }

            }

            }catch(error){
                return console.log(error)
            }

    },


    methods:{

        async likedOrNot(){
            try{
                const userId = localStorage.getItem("userId")
                const postId = this.postId
                const config = require('../config')

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){

                    if(data.movie.likes.includes(JSON.parse(userId))){
                        this.liked = true
                        
                    } else {
                        this.liked = false
                    }

                } else {

                    const res = await fetch(config.hostname + `api/serial/getOne/${postId}`)
                    const data = await res.json()

                    if(data.serial !== null){

                        if(data.serial.likes.includes(JSON.parse(userId))){
                            this.liked = true
                        } else {
                            this.liked = false
                        }
                    }
                }

            }catch(error){
                return console.log(error)
            }
        },


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
                return console.log(error)
            }
        },

        async like(postId, userId){
            try{
                const config = require('../config')

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){

                    const body = JSON.stringify({
                        postId,
                        userId
                    })


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
      

                return this.$router.go()

                } else {

                    const body = JSON.stringify({
                        postId,
                        userId
                    })

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
                return console.log(error)
            }
        },
        
        async addPost(){
            try{

                if(this.post.post === "movie"){
                    const userId = JSON.parse(localStorage.getItem("userId"))

                    const body = JSON.stringify({
                        title: this.post.title,
                        director: this.post.director,
                        description: this.post.description,
                        score: this.post.score,
                        userId: userId,
                        image: this.post.image
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

                    return this.$router.push(`/edit/${this.post._id}`)

                } else {
                    const userId = JSON.parse(localStorage.getItem("userId"))

                    const body = JSON.stringify({
                        title: this.post.title,
                        director: this.post.director,
                        description: this.post.description,
                        score: this.post.score,
                        userId: userId,
                        image: this.post.image
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

                    return this.$router.push(`/edit/${this.post._id}`)


                }



            }catch(error){
                return console.log(error)
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
                        


        //                 // console.log(this.post.title)
        //                 // console.log(this.post.director)
        //                 // console.log(this.post.description)
        //                 // console.log(this.post.score)
        //                 // console.log(userId)
        //                 // console.log(this.post.image)
        //                 // console.log(this.post.post)
                    
        //             const config = require('../config')

        //             const res = await fetch(config.hostname + 'api/movie/duplicate', {
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
        //         return console.log(error)
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