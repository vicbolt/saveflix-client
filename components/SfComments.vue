<template>
    <div class="sf-comments">

        <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>

        <v-divider class="mb-4"></v-divider>
            <v-card>
                    <v-alert v-if="this.msg" type="info" class="text-center" dismissible border="top" color="green" dark> {{this.msg}} </v-alert>

                <v-app-bar>
                    <v-icon class="mr-4" size="25"> mdi-comment-plus-outline </v-icon>
                    <v-app-bar-title class="font-weight-black mt-1" style="font-size:15px"> DEJAR UN COMENTARIO </v-app-bar-title>
                    <v-spacer></v-spacer>
                </v-app-bar>
                <v-card-text>
                    
                    <v-text-field v-model="message" outlined class="mt-1" placeholder="Escribe tu comentario"></v-text-field>
                    <v-btn color="rgb(229,9,20)" class="mt-n6" block @click="sendComment()"> ENVIAR </v-btn>
                </v-card-text>
                <v-divider class="mb-6" ></v-divider>

                <v-card-text>
                    <v-row v-for="comment in comments" :key="comment._id" class="mb-3" style="background-color: rgb(37, 37, 37 ); border-radius: 50px">
                        <v-col :cols="avatar" v-if="!xs">
                            <v-img class="imagen mr-1" :src="comment.user.avatar" width="100px" height="100px" />
                        </v-col>
                        <v-col :cols="content">
                            <v-row>
                                <v-col cols="6">
                                    <p class="font-weight-black mt-5"  style=" font-size: 17px; color:white; width: auto"> {{comment.user.username}} ha comentado: </p>
                                </v-col>
                                <v-col cols="5">
                                    <p  style="font-size:auto; text-align: right"> {{comment.date}} </p>
                                </v-col>
                                <v-col cols="1">
                                    <v-btn v-if="comment.user._id === userId || comment.post.userId === userId " @click="removeComment(comment._id)" class=" font-weight-black mt-n2" style="background-color: red; min-width:1px; width:30px; min-height:1px; height: 30px; border-radius:100px; text-align:center "> <v-icon class="" size="18px">mdi-trash-can-outline</v-icon></v-btn>
                                </v-col>
                            </v-row>
                            <v-row class="mt-n5">
                                <v-col cols="12">
                                    <p> {{comment.message}} </p>
                                </v-col>
                            </v-row>
                        </v-col> 
                    </v-row>

                </v-card-text>
            </v-card>

    </div>
</template>

<script>

export default ({
    data() {
        return{
            message: "",
            comments: [],
            ownerId: "",
            userId: "",
            error: "",
            msg: "",
            avatar: "2",
            content: "9",
            xs: false
            
        }
    },

    async beforeMount(){

        await this.getComments()
       
    },

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)
  
            if(value === "xs") {
                this.xs = true
                this.avatar = "12"
                this.content = "12"
               
            }
        }
    },

    methods: {

        async sendComment(){
            try{
                const config = require('../config')

                const postId = localStorage.getItem("postId")

                const userId = JSON.parse(localStorage.getItem('userId'))
                 
                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){

                    const body = JSON.stringify({
                        userId,
                        postId,
                        message: this.message
                    })

                    const res1 = await fetch(config.hostname + 'api/movieComment/create', {
                        method: 'post',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body,
                    })

                    const data = await res1.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    this.msg = 'El comentario ha sido enviado'
                    setTimeout(() => {
                        this.$router.go()
                    }, 2000);

                } else {
                    const userId = JSON.parse(localStorage.getItem('userId'))

                    const body = JSON.stringify({
                        userId,
                        postId,
                        message: this.message
                    })

                    const res2 = await fetch(config.hostname + 'api/serialComment/create', {
                        method: 'post',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body,
                    })

                    const data1 = await res2.json()
                    if(data1.error){
                        return this.error = data.error
                    }

                    this.msg = 'El comentario ha sido enviado'
                    setTimeout(() => {
                        this.$router.go()
                    }, 2000);
                }

            } catch(error){
                return console.log(error)
            }
        },

        async getComments(){

            try{
                const config = require('../config')

                const postId = localStorage.getItem("postId")

                const userId = localStorage.getItem("userId")
                
                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()
                

                if(data.movie !== null){

                    const res = await fetch(config.hostname + `api/movieComment/getComments/${postId}`)

                    const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    this.userId = JSON.parse(userId)

                    for(const comment of data.comments){
                        
                        this.comments.push(comment)
                    }
                
                    return 

                } else {

                    const res = await fetch(config.hostname + `api/serialComment/getComments/${postId}`)

                    const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    this.userId = JSON.parse(userId)

                    for(const comment of data.comments){
                        this.comments.push(comment)
                    }
                }

            } catch(error){
                return console.log(error)
            }
        },

        async removeComment(commentId){
            try{

                const config = require('../config')

                const postId = localStorage.getItem("postId")

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){
                    const res = await fetch(config.hostname + `api/movieComment/remove/${commentId}`, {
                        method: 'delete',
                        headers: {
                            'Content-Type': 'application/json'
                        }
                    })

                    const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    setTimeout(() => {
                        this.$router.go()
                    }, 2000);

                } else {
                    const res = await fetch(config.hostname + `api/serialComment/remove/${commentId}`, {
                        method: 'delete',
                        headers: {
                            'Content-Type': 'application/json'
                        }
                    })

                    const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    }

                    setTimeout(() => {
                        this.$router.go()
                    }, 2000);
                }

            }catch(error){
                return console.log(error)
            }
        },


    }
})

</script>

<style scoped>
.imagen{
    border: 3px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 2em;
}
</style>