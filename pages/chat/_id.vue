<template>
    <div class="sf-chat" v-if="visible">

        <v-row>
            <v-col :cols="chat">
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


                    <v-card height="300px" width="auto">

                        <div id="scroll" width="30px">
                            <v-row v-for="mensaje in mensajes" :key="mensaje._id" >
                                <v-col cols="6"></v-col>
                                <v-col cols="6">
                                    <v-row class="userIdContainer" v-if="userId === mensaje.userOne">
                                        <v-col cols="12">
                                            <v-row style="">
                                                <v-col cols="10">
                                                    <h4 style="text-align: right">{{usernameUser}}</h4>
                                                </v-col>
                                                <v-col cols="2" style="display: flex; flex-direction: column; align-content: flex-end; flex-wrap: wrap;">
                                                    <v-img id="imagen" :src="avatarUser" width="35px" height="35px" />
                                                </v-col>
                                            </v-row>
                                        </v-col>
                                        <v-col cols="12">
                                            <p style="display: flex; justify-content: flex-end; text-align: right"> {{mensaje.content}}</p>
                                        </v-col>
                                    </v-row>
                                </v-col>
                            
                                
                                <!-- <v-col cols="12" class="userIdContainer mb-2" v-if="userId === mensaje.userOne" >
                                    <v-col cols="6"></v-col>
                                    <v-col cols="6">
                                    <v-row style="justify-content:flex-end">
                                        <v-col cols="8"></v-col>
                                        <v-col cols="2" style="justify-content:flex-center">
                                                
                                        </v-col>
                                        <v-col cols="2" style=" display: flex; justify-content:center; flex-direction: column; flex-wrap: wrap;">
                                                
                                        </v-col>
                                    </v-row>
                                    </v-col>
                                    <v-row style="justify-content:flex-end">
                                        <p class="p1 mr-2"> </p>
                                    </v-row>
                                </v-col> -->




                                <div class="textToContainer mb-2" v-if="userId !== mensaje.userOne" >
                                    <v-row style="display: flex; justify-content: right;">
                                        <v-col cols="4">
                                            <v-img  class="mr-12" id="imagen" :src="avatar" width="35px" height="35px" /> 
                                        </v-col> 
                                        <v-col cols="8">
                                            <h4>{{username}}</h4>
                                        </v-col>
                                    </v-row>
                                    <p class="p2 ml-5 mb-2"> {{mensaje.content}} </p>
                                </div>
                            </v-row>  
                        </div>
                    </v-card>
                    <v-row>
                        <v-col :cols="input">
                            <v-text-field v-model="content" placeholder="Escribe un mensaje" class="ml-3" v-on:keyup.enter="createMsg"></v-text-field>
                        </v-col>
                        <v-col :cols="boton">
                            <v-btn  color="rgb(229,9,20)" class="mt-4" width="28px" @click="createMsg" > ENVIAR </v-btn>
                        </v-col>
                    </v-row>

                </v-card>

            </v-col> 

            <!-- END CHAT INPUT -->

            <v-col :cols="amigos">
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
            chat: "",
            amigos: "",
            input: "",
            boton: "",
            value: this.$vuetify.breakpoint.name,
        }
    },

    created(){
        this.responsive()
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

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)
            if(value == "xl" || value === "lg"){
                this.chat = "8"
                this.amigos = "4"
                this.input = "10"
                this.boton = "2"
            }
            if(value === "md" || value === "sm" || value === "xs") {
                this.chat = "12"
                this.amigos = "12"
                this.input = "12"
                this.boton = "12"
            } 
        }
    },


    methods: {

        responsive(){
            if(this.value == "xl" || this.value === "lg"){
                this.chat = "8"
                this.amigos = "4"
                this.input = "10"
                this.boton = "2"
            }
            if(this.value === "md" || this.value === "sm" || this.value === "xs") {
                this.chat = "12"
                this.amigos = "12"
                this.input = "12"
                this.boton = "12"
            } 
        },

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
    justify-content: right;
    font-size: auto;
    padding: 15px;
    border-radius: 10%;

}

.p2{
    width: auto;
    justify-content: left;
    font-size: auto;
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
    
    width: inherit;
    padding-top: 7px;
    background: rgb(36, 36, 36);
    border-radius: 10%;
}

.textToContainer{
    padding-top: 7px;
    width:fit-content;
    background: rgb(36, 36, 36);
    border-radius: 10%;
}

</style>
