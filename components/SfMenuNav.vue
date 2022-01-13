<template>
    <div class="sf-menu-nav" v-if="visible">

        <v-alert v-if="this.error" class="text-center" dismissible type="error" border="top" color="red" dark> {{this.error}} </v-alert>

        <v-app-bar class="barra" elevation="16" rounded >
            <a href="/">
            <img height="50em" width="130em" src="@/assets/images/logotipoWeb.png" />
            </a>
            <div class="">
                <v-row class="mt-2">
                    <v-col cols="1" class="mr-13 ml-n10">
                        <v-btn id="Btn" class="font-weight-black ml-15" width="10em" height="50px" @click="explorar()"> EXPLORAR </v-btn>
                    </v-col>
                    <v-col cols="1" class="mr-13 ml-13">    
                        <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="misPeliculas()">MIS PELICULAS</v-btn>
                    </v-col>
                    <v-col cols="1" class="mr-13">
                        <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="misSeries()">MIS SERIES</v-btn>
                    </v-col>
                    <v-col cols="1" class="mr-13">
                        <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="ranking()">MI RANKING</v-btn>
                    </v-col>
                    <v-col cols="1" class="mr-13">
                        <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="chat()">CHAT </v-btn>
                <!-- <v-badge color="pink" dot>CHAT</v-badge> -->
                    </v-col>
                    <v-col cols="1" class="mr-16">
                        <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="perfil()">MI PERFIL</v-btn>
                    </v-col>
                    <v-col cols="1" class="mt-3 ml-5">
                        <v-text-field v-model="search" width="10em" placeholder="USUARIO" type="text" v-on:keyup.enter="buscar"></v-text-field>
                    </v-col>
                    <v-col cols="1" class="mt-3">
                        <v-btn color="red" @click="buscar"> <v-icon size="20px">mdi-movie-search-outline</v-icon> </v-btn>
                    </v-col>
                </v-row>
               
            </div>

            <v-spacer></v-spacer>


  
            <v-menu :close-on-content-click="false" :nudge-width="400" max-width="250px">
                <template v-slot:activator="{ on }">
                    <v-btn class="mr-10" v-on="on" depressed fab small>
                        <v-avatar size="55" class="mr-3" color="rgb(229,9,20)"> 
                             <img class="avatar" :src="avatar" alt="avatar">
                        </v-avatar>
                    </v-btn>
                </template>

                <v-card>
                    <v-list>
                        <v-list-item>
                            <v-list-item-avatar size="60" class="mr-5" color="rgb(229,9,20)">
                                <img class="avatar" :src="avatar" alt="avatar">
                            </v-list-item-avatar>
                            <v-list-item-title>
                                <v-list-item-title class="font-weight-black"> {{username}} </v-list-item-title>
                                <v-list-item-subtitle> {{email}} </v-list-item-subtitle>
                            </v-list-item-title>
                        </v-list-item>
                    </v-list>

                    <v-divider class="mt-n2 mb-2"></v-divider>

                    <v-list>
                        <v-list-item>
                            <v-row>
                                <v-col cols="12">
                                    <v-btn class="ml-1" width="auto" height="50px" light block> AMIGOS</v-btn>
                                </v-col>
                                <v-col cols="12">
                                    <v-dialog width="500">
                                        <template v-slot:activator="{ }">
                                            <v-btn class="ml-1 mt-n3" width="auto" height="50px" light block @click="goToEditarPerfil()">EDITAR PERFIL</v-btn>
                                        </template>
                                    </v-dialog>

                                </v-col>
                                <v-col cols="12">
                                    <v-btn class="ml-1" width="auto" height="50px" color="rgb(229,9,20)" block @click="logOut">CERRAR SESION</v-btn>
                                </v-col>    
                            </v-row>
                        </v-list-item>
                    </v-list>
                </v-card>
            </v-menu>
      
        </v-app-bar>
    </div>
</template>

<script>
export default({
    data() {
        return{
            visible: true,
            username: "",
            email: "",
            userId: "",
            avatar: undefined,
            error: "",
            search: "",

        }
    },

    async beforeMount(){

        await this.loadInfo()

    },

    methods: {

        async loadInfo(){
            try{
                const token = localStorage.getItem('token')
                const config = require('../config')

                if(!token){
                    this.visible = true
                }

                const strId = localStorage.getItem('userId')
                const id = JSON.parse(strId)

                const res = await fetch(config.hostname + `api/user/getOne/${id}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                
                this.email = data.user.email
                this.username = data.user.username
                this.userId = strId
                this.avatar = data.user.avatar

            } catch(error){
                return console.log(error)
            }

        },

        misPeliculas(){
            const strId = localStorage.getItem('userId')

            const id = JSON.parse(strId)
            this.$router.push(`/misPeliculas/${id}`)

        },


        misSeries(){
            const strId = localStorage.getItem('userId')

            const id = JSON.parse(strId)

            this.$router.push(`/misSeries/${id}`)

        },


        async buscar(){
            try{
                const config = require('../config')

                const typeUsername = this.search
                const username = typeUsername.replace(/ /g, "").toUpperCase()

            
                //BUSCAR USUARIO
                const res = await fetch(config.hostname + `api/user/search/${username}`)
                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                const userId = JSON.parse(localStorage.getItem("userId"))

                if(data === userId){
                    this.$router.push(`/miPerfil/${data}`)
                } else{
                    this.$router.push(`/perfil/${data}`)
                }

                
            }catch(error){
                return console.log(error)
            }
        },


        explorar(){
            const strId = localStorage.getItem('userId')

            const id = JSON.parse(strId)

            this.$router.push(`/explorar/${id}`)

        },

        chat(){
            const strId = localStorage.getItem('userId')

            const id = JSON.parse(strId)

            this.$router.push(`/chat/${id}`)
        },

        ranking(){
            const strId = localStorage.getItem('userId')

            const id = JSON.parse(strId)

            this.$router.push(`/ranking/${id}`)
        },

        perfil(){

            const strId = localStorage.getItem('userId')

            const id = JSON.parse(strId)

            this.$router.push(`/miPerfil/${id}`)

        },

        logOut(){
            localStorage.removeItem('token')
            localStorage.removeItem('userId')
            
            this.$router.push('/logIn')
            
        },
        editarPerfil(){
            this.$route.router.go('/editarPerfil')
        },

        goToEditarPerfil(){
            this.$router.push('/editProfilePage')
        }
        
    }
})
</script>


<style scoped>

.avatar{
    padding: 2.2px;
}

.barra{
    background: rgb(0,0,0);
background: linear-gradient(90deg, rgba(0,0,0,1) 0%, rgba(39,39,39,1) 100%);
}

#Btn{
    background: none;
}


</style>