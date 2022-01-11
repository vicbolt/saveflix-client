<template>
    <div class="sf-details">

        <v-alert v-if="this.error" type="error" border="top" color="red" dark> {{this.error}} </v-alert>

        <v-card>
            <v-app-bar shaped style="background: white; color: black">
                <v-row>
                    <v-col cols="9">
                        <h2 class="font-weight-black mt-1" style="font-size:22px" ><v-icon class="mr-4 mb-1" size="35" color="black"> mdi-star </v-icon> {{this.title}} </h2>
                    </v-col>
                    
                    <v-col cols="3">
                    <v-btn v-if="admin"  @click="edit" class="mr-2"><v-icon color="green">mdi-movie-edit-outline</v-icon></v-btn>
                    <v-btn v-if="admin" @click="remove"><v-icon color="rgb(229,9,20)">mdi-trash-can-outline</v-icon></v-btn>
                    </v-col>
                </v-row>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3">
                                    <v-img height="420px" max-width="300px" style="border: 4px solid white" :src="this.image"></v-img>
                                    <v-btn @click="goTo">{{this.ownerUsername}}</v-btn>
                            </v-col>
                            <v-col cols="8">
                                    <h6 class="mb-1">DIRECTOR</h6>
                                    <v-divider class="mb-1" style="width: 160px;"></v-divider>
                                        <h3 class="mb-6"> {{this.director}}</h3>

                                    <h6 class="mb-1">SINOPSIS</h6>
                                    <v-divider class="mb-2" style="width: 560px;"></v-divider>
                                        <p style="max-width:560px; height:180px; text-align: justify; overflow: scroll" class="mb-10"> {{this.description}}</p>
                                    
                                    <h6 class="mb-1 mt-8">PUNTUACIÓN</h6>
                                    <v-divider class="mb-1" style="width: 560px;"></v-divider>
                                    <v-progress-linear color="rgb(229,9,20)" v-model="this.score" height="25" style="width: 560px;">
                                            <strong> {{ Math.ceil(this.score) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                    </v-progress-linear>

                                    <v-row class="mt-4" style="width: 740px;">
                                        <v-col cols="3" >
                                            <v-row >
                                                <v-btn class="ml-2" @click="like">
                                                    <v-icon v-if="this.liked" class="mr-4" size="20" color="red"> mdi-heart </v-icon>
                                                    <v-icon v-if="!this.liked" class="mr-4" size="20" color="white"> mdi-heart </v-icon>
                                                    <p class="ml-n3 mt-4">{{this.likes.length}}</p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-row>
                                                <v-btn disabled>
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-eye </v-icon>
                                                    <p class="ml-n3 mt-4">{{this.views}}</p>
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
            
            liked: "",

            title: "",
            searchTitle: "",
            director: "",
            description: "",
            score: "",
            ownerId: "",
            ownerUsername: "",
            image: "",
            postId: "",
            likes: "",
            views: "",
            paramsId: "",
            error: "",
        }
    },


    async beforeMount(){

            await this.showDetails()
            await this.loadViews()
            await this.adminOrNot()
            await this.likedOrNot()

            //TODO TOKEN

    },


    methods:{

        async showDetails(){
            try{

                const config = require('../config')

                const postId =  localStorage.getItem("postId")

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null ){

                    this.postId = data.movie.postId
                    this.title = data.movie.title
                    this.searchTitle = data.movie.searchTitle
                    this.director = data.movie.director
                    this.description = data.movie.description
                    this.score = data.movie.score
                    this.ownerId = data.movie.userId._id
                    this.ownerUsername = data.movie.userId.username
                    this.image = data.movie.image
                    this.post = data.movie.post
                    this.likes = data.movie.likes
                    this.views = data.movie.views

                } else {
                    const res = await fetch(config.hostname + `api/serial/getOne/${postId}`)
                    const data = await res.json()

                    if(data.error){
                        return console.log(data.error)
                    }

                    this.postId = data.serial.postId
                    this.title = data.serial.title
                    this.searchTitle = data.serial.searchTitle
                    this.director = data.serial.director
                    this.description = data.serial.description
                    this.score = data.serial.score
                    this.ownerId = data.serial.userId._id
                    this.ownerUsername = data.serial.userId.username
                    this.image = data.serial.image
                    this.post = data.serial.post
                    this.likes = data.serial.likes
                    this.views = data.serial.views
                }

            }catch(error){
                return console.log(error)
            }
        },

        async loadViews(){
            try{
            const config = require('../config')

            const postId =  localStorage.getItem("postId")

            const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
            const data = await res.json()

                if(data.movie !== null){

                    const body = JSON.stringify({
                        postId: postId
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

                    this.views = data.post.views

                } else {

                    const body = JSON.stringify({
                    postId: postId
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

                    this.views = data.post.views
                }

            } catch(error){
                return console.log(error)
            }
        },

        async adminOrNot(){
            try{

                const strUserId = localStorage.getItem('userId')

                const userId = JSON.parse(strUserId)

                const ownerId = this.ownerId

                if(userId === ownerId){
                    this.admin = true
                    this.admin2 = false
                }

            }catch(error){
                return console.log(error)
            }
        },

        async likedOrNot(){
            try{
                const userId = localStorage.getItem("userId")
                const postId = localStorage.getItem("postId")
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


        async remove(){
            try{

                const config = require('../config')

                const postId = localStorage.getItem("postId")

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
                        return this.error = data.error
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
                        return this.error = data.error
                    }

                    alert('El post ha sido borrado con éxito')
                    this.$router.push('/misSeries')
                }

            }catch(error){
                return console.log(error)
            }
        },

        async like(){
            try{

                const config = require('../config')

                const userId = JSON.parse(localStorage.getItem("userId"))
                const postId = localStorage.getItem("postId")

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
                        return this.error = data.error
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
                        return this.error = data.error
                    }

                    return this.$router.go()
            }

            } catch(error){
                return console.log(error)
            }
        },
        
        async addPost(){
            try{
                const config = require('../config')

                const postId = localStorage.getItem("postId")

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){

                    const userId = JSON.parse(localStorage.getItem("userId"))

                    const body = JSON.stringify({
                        title: this.title,
                        searchTitle: this.searchTitle,
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

                    const id = data.movie._id

                    return this.$router.push(`/edit/${id}`)

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

                    const id = data.serial._id

                    return this.$router.push(`/edit/${id}`)
                }

            }catch(error){
                return console.log(error)
            }
        },


        edit(){

            const postId = localStorage.getItem("postId")

            this.$router.push(`/edit/${postId}`)
        },

        goTo(){
            
            const ownerId = this.ownerId

            localStorage.setItem("ownerId", ownerId)
            
            const userId = JSON.parse(localStorage.getItem("userId"))

            if(ownerId === userId){
                return this.$router.push(`/miPerfil/${userId}`)
            }

            return this.$router.push(`/perfil/${ownerId}`)
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