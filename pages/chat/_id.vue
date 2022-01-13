<template>
    <div class="sf-chat" v-if="visible">
        <v-row>

            <!-- CHAT INPUT -->
            <v-col cols="8">
                <v-card elevation="2" shaped class="mb-5">
                    <v-app-bar>
                        <v-row>
                            <v-col cols="2">
                                <v-img class="ml-n1" id="imagen" src="https://previews.123rf.com/images/jemastock/jemastock1705/jemastock170506999/78062109-cara-de-joven-aislado-sobre-fondo-blanco-expresi%C3%B3n-de-la-cara-hermosa-del-muchacho-ilustraci%C3%B3n-vecto.jpg" width="50px" height="50px" />
                            </v-col>
                            <v-col cols="10">
                                <h3 class="font-weight-black mt-3 ml-n10"> PEDRO </h3>
                            </v-col>
                        </v-row>
                    </v-app-bar>
                    <v-card height="300px">
                        <div>
                            <p>hola</p>
                        </div>
                    </v-card>
                    <v-row>
                        <v-col cols="10">
                            <v-text-field v-model="message" placeholder="Escribe un mensaje" class="ml-3"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn  color="rgb(229,9,20)" class="mt-4" width="28px"> ENVIAR </v-btn>
                        </v-col>
                    </v-row>

                </v-card>
            </v-col>

            <!-- END CHAT INPUT -->

            <v-col cols="4">
                <v-card elevation="2" shaped class="mb-5">
                    <v-app-bar>
                        <v-icon> mdi-account-group</v-icon>
                        <v-card-title class="font-weight-black ml-5"> LISTA DE AMIGOS</v-card-title>
                    </v-app-bar>
                <v-row class="mt-2" v-for="follower in following" :key="follower._id">
                    <v-col cols="4" >
                        <v-img id="imagen" class="ml-n" :src="follower.avatar" width="50px" height="50px" />
                    </v-col>
                    <v-col cols="8">
                        <p class="font-weight-black mt-3">{{follower.username}}</p>
                        <v-divider class="mt-3 mb-3"></v-divider>
                    </v-col>
                </v-row>
                

                </v-card>
            </v-col>

        </v-row>

    </div>
</template>

<script>
export default ({
    layout: 'soloMenu',
    
    data() {
        return{
            visible: true,
            following: [],
            message: "",
        }
    },

   async beforeMount(){

        await this.loadFollowing()

        const token = localStorage.getItem('token')

        if(!token){
            this.visible = false
        }
    },

    mounted() {
        this.socket = this.$nuxtSocket({
            channel: '/index'
        })

        /* Listen for events: */
        // this.socket.on('connection', (socket) => {
        //     console.log("socket", socket._id)
        // })
    },

    methods: {

        // emit() {
        //     this.socket.emit('message', {
        //         message: this.message,
        //         username: this.follower.username
        //     })
        // },

        async loadFollowing(){

            try{
                const config = require('/config')
                const id = JSON.parse(localStorage.getItem("userId"))

                

                const res = await fetch(config.hostname + `api/user/following/${id}`)

                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }

                const siguiendo = data.seguidores


                for(const follower of siguiendo){
                    this.following.push(follower)
                }

            }catch(error){
                return console.log(error)
            }
        },
    }
})
</script>

<style scoped>
#imagen{
    border: 2px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 2em;
}
</style>
