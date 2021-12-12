<template>
    <div class="sf-mi-perfil">
        <!-- BARRA DE TITULO -->
        <v-card class="mt-5">
                <v-row style="background: linear-gradient(180deg, red, rgb(209, 53, 53)">
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
                            <v-col cols="4"> 
                                <h3 style="text-align: center">{{this.allPosts}}</h3>
                                <h4 style="text-align: center">Publicaciones</h4>
                            </v-col>
                            <v-col cols="4" class="ml-n5 mr-n5" @click="goToFollowers()"> 
                                <h3 style="text-align: center">{{this.followers}}</h3>
                                <h4 style="text-align: center">Seguidores</h4>
                            </v-col>
                            <v-col cols="4" @click="goToFollowing()"> 
                                <h3 style="text-align: center">{{this.following}}</h3>
                                <h4 style="text-align: center">Siguiendo</h4>
                            </v-col>
                            
                        </v-row>
                    </v-col>
                    <v-col cols="1">
                        <v-btn color="white" style="color:black" class="mt-5">EDITAR PERFIL</v-btn>
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
                                <v-col cols=4  v-for="movie in movies" :key="movie.id" @click="goTo(movie._id)">
                                    <v-list-item>
                                        <v-list-item-content style="cursor: pointer" class="fondo">
                                            <v-img height="100px" style="border: 2px solid white" :src="movie.image"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{movie.title}} </v-list-item-title>
                                                    <v-progress-linear color="rgb(229,9,20)" v-model="movie.score" height="25">
                                                        <strong> {{ Math.ceil(movie.score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                                </v-progress-linear>
                                        </v-list-item-content>
                                    </v-list-item>
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
                                <v-col @click="goTo(serial._id)" cols=4 v-for="serial in serials" :key="serial.id">
                                    <v-list-item>
                                        <v-list-item-content style="cursor: pointer" class="fondo">
                                            <v-img height="100px" style="border: 2px solid white" :src="serial.image" />
                                            <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{serial.title}} </v-list-item-title>
                                                <v-progress-linear color="rgb(229,9,20)" v-model="serial.score" height="25">
                                                    <strong> {{ Math.ceil(serial.score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                                    </v-progress-linear>
                                        </v-list-item-content>
                                    </v-list-item>
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
            movies: [],
            serials: [],
            username: "",
            following: "",
            followers: "",
            avatar: undefined,
            allPosts: ""

        }
    },

    async beforeMount(){

        await this.loadMovies()
        await this.loadSerials()
        await this.loadUserDetails()
        
    },

    // beforeDestroy(){
    //     localStorage.removeItem("ownerId")
    // },

    methods:{
        async loadSerials(){
            try{

                const config = require('/config')
                const strParamsId = localStorage.getItem('userId')

                const paramsId = JSON.parse(strParamsId)

                const res = await fetch(config.hostname + `api/serial/getAll/${paramsId}`)

                const data = await res.json()
                if(data.error){
                    return alert(data.error)
                }

                this.serials = []
                for(const serial of data.serials){
                    this.serials.push(serial)
                }

            }catch(error){
                return res.json(error)
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
                    return alert(data.error) 
                }
                

                this.movies = []
                for(const movie of data.movies){
                    this.movies.push(movie)
                }


            }catch(error){
                return res.json(error)
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
                    return alert(data.error)
                }

                const followers = data1.seguidores

                this.followers = followers.length

                const posts = this.movies.concat(this.serials)

                this.allPosts = posts.length


            }catch(error){
                return res.json(error)
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
    }

})

</script>

<style scoped>
.avatar{
    padding: 2.2px;
}
</style>