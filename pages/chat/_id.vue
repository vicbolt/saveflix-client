<template>
    <div class="sf-chat" v-if="visible">
        <v-row>

            <!-- CHAT INPUT -->
            <v-col cols="8">
                <v-card elevation="2" shaped class="mb-5">
                    <v-app-bar>
                        <v-row>
                            <v-col cols="2">
                                <v-img class="ml-n1" id="imagen" :src="avatar" width="50px" height="50px" />
                            </v-col>
                            <v-col cols="10">
                                <h3 class="font-weight-black mt-3 ml-n10"> {{this.username}} </h3>
                            </v-col>
                        </v-row>
                    </v-app-bar>
                    <v-card height="300px">
                        <div>
                            <v-row v-for="mensaje in mensajes" :key="mensaje._id">
                                <h4 style="align: right"> {{mensaje.content}} </h4>
                            </v-row>
                            
                        </div>
                    </v-card>
                    <v-row>
                        <v-col cols="10">
                            <v-text-field v-model="content" placeholder="Escribe un mensaje" class="ml-3"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn  color="rgb(229,9,20)" class="mt-4" width="28px" @click="createMsg"> ENVIAR </v-btn>
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
                    <v-row class="mt-2" v-for="follower in following" :key="follower._id" @click="textTo(follower.username, follower.avatar, follower._id)">
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
            mensajes: [],
            username: "",
            avatar: "",
            content: "",
        }
    },

   async beforeMount(){

        await this.loadFollowing()

        await this.loadMsg()

        const token = localStorage.getItem('token')

        if(!token){
            this.visible = false
        }
    },

    methods: {

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

        async textTo(username, avatar, id){

            await this.loadMsg()

            this.username = username
            this.avatar = avatar

            localStorage.setItem("textTo", id )

        },

        async loadMsg(){
            try{

                this.mensajes = []
                const config = require('/config')

                const userOne = JSON.parse(localStorage.getItem("userId"))
                const userTwo = localStorage.getItem("textTo")

                console.log(userOne, userTwo)

                const res = await fetch(config.hostname + `api/msg/getAll/${userOne}/${userTwo}`)

                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }

                for(const mensaje of data.mensajes){
                    this.mensajes.push(mensaje)
                }

            }catch(error){
                return console.log(error)
            }
        },

        async createMsg(){
            try{

                const config = require('/config')

                const userOne = JSON.parse(localStorage.getItem("userId"))
                const userTwo = localStorage.getItem("textTo")
                const content = this.content

                console.log(userOne, userTwo, this.content)

                const body = JSON.stringify({
                    userOne,
                    userTwo,
                    content
                })

                const res = await fetch(config.hostname + 'api/msg/create', {
                    method: 'post',
                    mode: "cors",
                    headers: {
                        'Content-type' : 'application/json',
                        'Access-Control-Allow-Origin': '*'
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }

                await this.loadMsg()
                this.content = ""

            }catch(error){
                return console.log(error)
            }
        }
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
