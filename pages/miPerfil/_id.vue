<template>
    <div class="sf-mi-perfil" v-if="visible">

        <v-alert v-if="this.error" type="error" border="top" color="red" dark> {{this.error}} </v-alert>
        <!-- BARRA DE TITULO -->
        <v-card class="mt-5">
                <v-row style="background: linear-gradient(180deg, rgb(229,9,20), rgb(209, 53, 53)">
                    <v-col cols="5"  style="background-color: ; ">
                        <v-row>
                            <v-avatar size="100" class="ml-6 mr-3 p-4" color="rgb(209, 53, 53)"> 
                                <img class="avatar" :src="avatar" alt="avatar">
                            </v-avatar>
                            <h2 class="mt-8"> {{this.username}} </h2>
                        </v-row>                       
                    </v-col>
                    <v-col cols="5">
                        <v-row class="mt-1">
                            <v-col cols="4" style="cursor: pointer"> 
                                <h3 style="text-align: center">{{this.allPosts}}</h3>
                                <h4 style="text-align: center">Publicaciones</h4>
                            </v-col>
                            <v-col cols="4" style="cursor: pointer" class="ml-n5 mr-n5" @click="goToFollowers()"> 
                                <h3 style="text-align: center">{{this.followers}}</h3>
                                <h4 style="text-align: center">Seguidores</h4>
                            </v-col>
                            <v-col cols="4" style="cursor: pointer" @click="goToFollowing()"> 
                                <h3 style="text-align: center">{{this.following}}</h3>
                                <h4 style="text-align: center">Siguiendo</h4>
                            </v-col>
                            
                        </v-row>
                    </v-col>
                    <v-col cols="1">
                        <v-btn color="white" style="color:black" class="mt-5" @click="goToEditProfilePage()">EDITAR PERFIL</v-btn>
                    </v-col>
                </v-row>
        </v-card>
        <!-- MIS PELICULAS Y MIS SERIES -->
        <v-card>
            <v-row class="mt-5">
                <v-col cols="6">
                    <v-card elevation="2">
                        <v-app-bar shaped style=" background-color:white; color: black">
                            <v-icon class="mr-4" size="35" color="black"> mdi-movie-open-star-outline </v-icon>
                            <v-card-title class="font-weight-black mt-1" style="font-size:20px; color:black"> MIS PELICULAS </v-card-title>
                        </v-app-bar>
                        <v-card-text> 
                            <v-row>
                                <v-col cols="3"  v-for="movie in movies" :key="movie.id" @click="goToDetails(movie._id)" style="cursor: pointer" >
                                    <v-img height="160px" style="border: 2px solid white" :src="movie.image"/>
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{movie.title}} </v-list-item-title>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>


                <v-col cols="6">
                    <v-card elevation="2">
                        <v-app-bar shaped style=" background-color:white">
                            <v-icon class="mr-4" size="35" color="black"> mdi-television-classic </v-icon>
                            <v-card-title class="font-weight-black mt-1" style="font-size:20px; color:black"> MIS SERIES </v-card-title>
                        </v-app-bar>

                        <v-card-text>
                            <v-row>
                                <v-col @click="goToDetails(serial._id)" cols="3" v-for="serial in serials" :key="serial.id"  style="cursor: pointer">
                                    <v-img height="160px" style="border: 2px solid white" :src="serial.image" />
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{serial.title}} </v-list-item-title>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>
                <v-col cols="6">
                    <v-card elevation="2">
                        <v-app-bar shaped style=" background-color:white">
                            <v-icon class="mr-4" size="35" color="black"> mdi-television-classic </v-icon>
                            <v-card-title class="font-weight-black mt-1" style="font-size:20px; color:black"> PELICULAS PENDIENTES </v-card-title>
                        </v-app-bar>

                        <v-card-text>
                            <v-row>
                                <v-col @click="goToDetailsPendiente(movieP._id)" cols="3" v-for="movieP in moviesP" :key="movieP.id"  style="cursor: pointer">
                                    <v-img height="160px" style="border: 2px solid white" :src="movieP.image" />
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{movieP.title}} </v-list-item-title>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>
                    <v-col cols="6">
                    <v-card elevation="2">
                        <v-app-bar shaped style=" background-color:white">
                            <v-icon class="mr-4" size="35" color="black"> mdi-television-classic </v-icon>
                            <v-card-title class="font-weight-black mt-1" style="font-size:20px; color:black"> SERIES PENDIENTES </v-card-title>
                        </v-app-bar>

                        <v-card-text>
                            <v-row>
                                <v-col  cols="3"  @click="goToDetailsPendiente(serialP._id)" v-for="serialP in serialsP" :key="serialP.id" style="cursor: pointer">
                                    <v-img height="160px" style="border: 2px solid white" :src="serialP.image" />
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{serialP.title}} </v-list-item-title>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>
            </v-row>
        </v-card>

    </div>
</template>



<script>

export default({
    layout: 'soloMenu',

    data(){
        return{
            visible: true,
            movies: [],
            serials: [],
            moviesP: [],
            serialsP: [],
            username: "",
            following: "",
            followers: "",
            avatar: undefined,
            allPosts: "",
            error: "",

        }
    },

    async beforeMount(){

        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("logIn")
        }

        await this.loadMovies()
        await this.loadSerials()
        await this.loadUserDetails()
        await this.loadMoviesP()
        await this.loadSerialsP()
        
    },

    methods:{
        async loadSerials(){
            try{

                const config = require('/config')
                const strParamsId = localStorage.getItem('userId')

                const paramsId = JSON.parse(strParamsId)

                const res = await fetch(config.hostname + `api/serial/getAll/${paramsId}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                this.serials = []
                for(const serial of data.serials){
                    this.serials.push(serial)
                }

            }catch(error){
                return console.log(error)
            }
        },

        async loadMovies(){
            try{

                const config = require('/config')

                const strParamsId = localStorage.getItem('userId')

                const paramsId = JSON.parse(strParamsId)

                const res = await fetch(config.hostname + `api/movie/getAll/${paramsId}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }
                

                this.movies = []
                for(const movie of data.movies){
                    this.movies.push(movie)
                }


            }catch(error){
                return console.log(error)
            }
        },

        async loadUserDetails(){
            try{

                const config = require('/config')

                const strParamsId = localStorage.getItem('userId')
                const userId = JSON.parse(strParamsId)

                const res = await fetch(config.hostname + `api/user/getOne/${userId}`)

                const data = await res.json()
                if(data.error){
                        console.log(data.error)
                }

                const user = data.user

                this.username = user.username
                this.avatar = user.avatar
                this.following = user.following.length
                this.userId = user._id

                console.log(this.userId)

                const res1 = await fetch(config.hostname + `api/user/followers/${userId}`)

                const data1 = await res1.json()
                if(data1.error){
                    return this.error = data.error
                }

                const followers = data1.seguidores

                this.followers = followers.length

                const posts = this.movies.concat(this.serials)

                this.allPosts = posts.length


            }catch(error){
                return console.log(error)
            }
        },

        async loadMoviesP(){
            try{

                const userId = JSON.parse(localStorage.getItem("userId"))

                const config = require('/config')

                const res = await fetch(config.hostname + `api/moviePendiente/getAll/${userId}`)

                const data = await res.json()
                if(data.error){
                    return console.log(data.error)
                }

                const moviesP = data.moviesPendientes

                for(const movieP of moviesP){
                    this.moviesP.push(movieP)
                }



            }catch(error){
                return console.log(error)
            }
        },


        async loadSerialsP(){
            try{

                const userId = JSON.parse(localStorage.getItem("userId"))

                const config = require('/config')

                const res = await fetch(config.hostname + `api/serialPendiente/getAll/${userId}`)

                const data = await res.json()
                if(data.error){
                    return console.log(data.error)
                }

                const serialsP = data.serialsP

                for(const serialP of serialsP){
                    this.serialsP.push(serialP)
                }



            }catch(error){
                return console.log(error)
            }
        },

        goToFollowers(){

            localStorage.setItem("ownerId", this.userId)

            this.$router.push(`/followers/${this.userId}`)
        },

        goToFollowing(){

            localStorage.setItem("ownerId", this.userId)
            this.$router.push(`/following/${this.userId}`)
        },

        goToDetails(id){
            localStorage.setItem("postId", id)
            this.$router.push(`/details/${id}`)
        },

        goToDetailsPendiente(id){

            this.$router.push(`/pendienteDetails/${id}`)
        },

        goToEditProfilePage(){

            this.$router.push('/editProfilePage')
        }
    }

})

</script>

<style scoped>
.avatar{
    padding: 2.2px;
}
</style>