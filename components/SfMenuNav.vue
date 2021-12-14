<template>
    <div class="sf-menu-nav" v-if="visible">

        <v-app-bar class="barra" elevation="16" rounded >
            <a href="/">
            <img height="50em" width="130em" src="@/assets/images/logotipoWeb.png" />
            </a>
            <div class="ml-5px">
            <v-btn id="Btn" class="font-weight-black ml-15" width="10em" height="50px" @click="explorar()">
            <v-badge color="pink" dot>EXPLORAR</v-badge>
            </v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="misPeliculas()">MIS PELICULAS</v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="misSeries()">MIS SERIES</v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="ranking()">MI RANKING</v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="chat()">
            <v-badge color="pink" dot>CHAT</v-badge>
            </v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" @click="perfil()">MI PERFIL</v-btn>
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
                                        <template v-slot:activator="{ on, attrs }">
                                            <v-btn class="ml-1 mt-n3" width="auto" height="50px" light block @click="editarPerfil" v-bind="attrs" v-on="on">EDITAR PERFIL</v-btn>
                                        </template>
                                        <SfEditarPerfil/>
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
            avatar: undefined
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
                    return alert(data.error)
                }

                
                this.email = data.user.email
                this.username = data.user.username
                this.userId = strId
                this.avatar = data.user.avatar

            } catch(error){
                return alert(error)
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