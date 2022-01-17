<template>
    <div class="sf-perfil" v-if="visible">

        <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>

        <!-- BARRA DE TITULO -->
        <v-card class="mt-5" shaped>
                <v-row style="background: linear-gradient(180deg, rgb(229,9,20), rgb(209, 53, 53); height: 130px">
                    <v-col cols="5">
                        <v-row>
                            <v-avatar size="100" class="ml-6 mr-5 mt-4 " color="rgb(209, 53, 53)"> 
                                <img class="avatar" :src="avatar" alt="avatar">
                            </v-avatar>
                            <h2 class="mt-10"> {{this.username}} </h2>
                        </v-row>                       
                    </v-col>
                    <v-col cols="5">
                        <v-row class="mt-5">
                            <v-col cols="4" > 
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
                        <v-btn v-if="!siguiendo" @click="follows()" color="green"  class="mt-10" style="color:black">SEGUIR</v-btn>
                        <v-btn v-if="siguiendo"  @click="follows()" color="rgb(229,9,20)" class="mt-10 ml-n8" style="color:white; border: 2px solid white">DEJAR DE SEGUIR</v-btn>
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
                            <v-card-title class="font-weight-black mt-1" style="font-size:20px; color:black"> PELICULAS </v-card-title>
                        </v-app-bar>
                        <v-card-text>
                            <v-row>
                                <v-col cols="3"  v-for="movie in movies" :key="movie.id" @click="goTo(movie._id)" style="cursor: pointer" >
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
                            <v-card-title class="font-weight-black mt-1" style="font-size:20px; color:black"> SERIES </v-card-title>
                        </v-app-bar>

                        <v-card-text>
                            <v-row>
                                <v-col @click="goTo(serial._id)" cols="3" v-for="serial in serials" :key="serial.id" style="cursor: pointer">
                                    <v-img height="160px" style="border: 2px solid white" :src="serial.image" />
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{serial.title}} </v-list-item-title>
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
            username: "",
            following: "",
            followers: "",
            avatar: undefined,
            allPosts: "",
            ownerId: "",
            siguiendo: false,
            error: "",


        }
    },

    async beforeMount(){
        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")
        }
        
        await this.loadUserDetails()
        await this.loadMovies()
        await this.loadSerials()
    },

    methods:{

        async loadSerials(){
            try{
                const config = require('/config')
                const id = localStorage.getItem('ownerId')


                const res = await fetch(config.hostname + `api/serial/getAll/${id}`)

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

                const id = localStorage.getItem('ownerId')

                const res = await fetch(config.hostname + `api/movie/getAll/${id}`)

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

                const ownerId = localStorage.getItem('ownerId')

                const res = await fetch(config.hostname + `api/user/getOne/${ownerId}`)

                const data = await res.json()
                if(data.error){
                        console.log(data.error)
                }

                const user = data.user

                this.username = user.username
                this.avatar = user.avatar
                this.following = user.following.length
                this.ownerId = ownerId

                if(user.following.includes(userId)){
                    this.siguiendo = true
                } else{
                    this.siguiendo = false
                }

                const res1 = await fetch(config.hostname + `api/user/followers/${ownerId}`)

                const data1 = await res1.json()
                if(data1.error){
                    return this.error = data.error
                }

                const userId = JSON.parse(localStorage.getItem("userId"))
                const followers = data1.seguidores

                if(followers.length > 0){
                    console.log(followers[0]._id === userId )
                    for(let i = 0; i < followers.length; i++){
                        if(followers[i]._id === userId){
                            this.siguiendo = true
                        }
                    }
                }

                this.followers = followers.length

                const posts = this.movies.concat(this.serials)

                this.allPosts = posts.length

            }catch(error){
                return console.log(error)
            }
        },

        async follows(){
            try{

                const config = require('/config')

                const ownerId = localStorage.getItem("ownerId")

                const userId = JSON.parse(localStorage.getItem("userId"))

                const body = JSON.stringify({
                    ownerId: ownerId,
                    userId: userId,
                })

                const res = await fetch(config.hostname + 'api/user/follow', {
                    method: "post",
                    headers: {
                        'Content-type':'application/json',
                    },
                    // mode: "no-cors",
                    body: body,
                })

                const data = await res.json()
                if(data.error){
                    this.$router.go()
                    return this.error = data.error
                }

                this.$router.go()
                alert('Ahora sigues a este usuario')
                


            }catch(error){
                return console.log(error)
            }
        },

        goToFollowers(){

            localStorage.setItem("ownerId", this.ownerId)

            this.$router.push(`/followers/${this.ownerId}`)
        },

        goToFollowing(){

            localStorage.setItem("ownerId", this.ownerId)
            this.$router.push(`/following/${this.ownerId}`)
        },
    }

})

</script>

<style scoped>
.avatar{
    padding: 2.2px;
    border: 2px solid white;;
}
</style>