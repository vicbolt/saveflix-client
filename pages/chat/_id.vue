<template>
    <div class="sf-chat" v-if="visible">
        <v-row>

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
                        <div id="scroll">
                            <v-row class="" v-for="mensaje in mensajes" :key="mensaje._id">
                                <div class="userIdContainer mb-2" v-if="userId === mensaje.userOne" style="width:760px; align-content:right">
                                    <v-row >
                                        <v-col cols="9"></v-col>
                                        <v-col cols="3" style="align-content:right">
                                            <v-row>
                                                <v-col cols="4" >
                                                <v-img class="" id="imagen" :src="avatarUser" width="35px" height="35px" style="align-items:right" /> 
                                                </v-col> 
                                                <v-col cols="8" class="ml-n10">
                                                <h4 style="text-align: right">{{usernameUser}}</h4>
                                                </v-col>
                                            </v-row>
                                        </v-col>
                                    </v-row>
                                    <p class="p1">{{mensaje.content}} </p>
                                </div>
                                <div class="textToContainer mb-2" v-if="userId !== mensaje.userOne" style="width:760px; align-content: left">
                                    <v-row >
                                        <v-col cols="4">
                                            <v-row>
                                                <v-col cols="4">
                                                <v-img  id="imagen" :src="avatar" width="35px" height="35px" /> 
                                                </v-col> 
                                                <v-col cols="8">
                                                <h4>{{username}}</h4>
                                                </v-col>
                                            </v-row>
                                        </v-col>
                                        <v-col cols="8"></v-col>
                                    </v-row>
                                    <p class="p2 mb-3"> {{mensaje.content}} </p>
                                </div>
                            </v-row>  
                        </div>
                    </v-card>
                    <v-row>
                        <v-col cols="10">
                            <v-text-field v-model="content" placeholder="Escribe un mensaje" class="ml-3" v-on:keyup.enter="createMsg"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn  color="rgb(229,9,20)" class="mt-4" width="28px" @click="createMsg" > ENVIAR </v-btn>
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
            userId: "",
            avatarUser: "",
            usernameUser: "",
            msgSize: "",
            msgSizeUpdated: "",
        }
    },

   async beforeMount(){

        const token = localStorage.getItem('token')
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")

        }

        const userId = JSON.parse(localStorage.getItem("userId"))
        this.userId = userId

        await this.loadUser()

        await this.loadFollowing()

    },

    // watch: {
    //     '$msgSize'(value){
    //         if(value < this.msgSizeUpdated){
    //             console.log("funsionaa")
    //             this.loadMsg()
    //         }
    //     }
    // },

    methods: {

        toScroll(){
            const div = this.$el.querySelector("#scroll");
            const altura = this.$el.querySelector("#scroll").scrollHeight

            console.log(div, altura)
            div.scrollTop = altura
        },

        async loadUser(){
            try{
                const config = require('/config')

                const res = await fetch(config.hostname + `api/user/getOne/${this.userId}`)
                const data = await res.json()
                if(data.error){
                    return console.log(data.error)
                }

                this.avatarUser = data.user.avatar
                this.usernameUser = data.user.username
            
            }catch(error){
                return console.log(error)
            }
        },

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

            await this.backLoadMsg()

            this.toScroll()

            this.username = username
            this.avatar = avatar

            localStorage.setItem("textTo", id )

            setInterval(async () => {
                await this.backLoadMsg()}
            , 2000)

        },

        async loadMsg(){
            try{

                this.mensajes = []
                const config = require('/config')

                const userOne = JSON.parse(localStorage.getItem("userId"))
                const userTwo = localStorage.getItem("textTo")

                const res = await fetch(config.hostname + `api/msg/getAll/${userOne}/${userTwo}`)

                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }

                this.msgSize = data.mensajes.length

                console.log("size: ", this.msgSize)

                for(const mensaje of data.mensajes){
                    this.mensajes.push(mensaje)
                }

            }catch(error){
                return console.log(error)
            }
        },

        async backLoadMsg(){
            try{
                
                const config = require('/config')

                const userOne = JSON.parse(localStorage.getItem("userId"))
                const userTwo = localStorage.getItem("textTo")

                const res = await fetch(config.hostname + `api/msg/getAll/${userOne}/${userTwo}`)
                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }

                this.msgSizeUpdated = data.mensajes.length
                console.log("msgSizeUpdated: ", this.msgSizeUpdated)

                if(this.msgSize < this.msgSizeUpdated){
                    await this.loadMsg()
                    this.toScroll()
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

.p1 {
    width: auto;

    text-align: right;

    padding: 15px;
    border-radius: 10%;

}

.p2{
    width: auto;
    text-align: left;
    padding: 15px;
    border-radius: 10%;

}

#scroll{
    width: 760px;
    max-height: 300px;
    height: 100%;
    overflow: scroll
}

.userIdContainer{
    padding-top: 7px;
    background: rgb(36, 36, 36);
    border-radius: 10%;
}

.textToContainer{
    padding-top: 7px;
    background: rgb(36, 36, 36);
    border-radius: 10%;
}

</style>
