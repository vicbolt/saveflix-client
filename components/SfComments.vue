<template>
    <div class="sf-comments">

        <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>

        <v-divider class="mb-4"></v-divider>
            <v-card>
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
                        <v-col cols="2">
                            <v-img class="imagen ml-4" :src="comment.user.avatar" width="80px" height="80px" />
                        </v-col>
                        <v-col cols="10">
                            <v-row>
                            <p class="font-weight-black mt-5"  style=" font-size: 17px; color:white; width: auto"> {{comment.user.username}}</p> <p class="font-weight-black mt-5 ml-2">ha comentado:</p>
                            <v-spacer></v-spacer>
                            <p class="mt-5 mr-5" style="font-size:11px"> {{comment.date}} </p>
                            <v-btn v-if="comment.user._id === userId || comment.post.userId === userId " @click="removeComment(comment._id)" class=" font-weight-black mt-n2" style="background-color: red; min-width:1px; width:30px; min-height:1px; height: 30px; border-radius:100px; text-align:center "> <v-icon class="" size="18px">mdi-trash-can-outline</v-icon></v-btn>
                            </v-row>
                            <p class=""> {{comment.message}} </p>
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
        }
    },

    async beforeMount(){


        await this.getComments()
       
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

                    return this.$router.go()

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

                    alert('El comentario ha sido enviado')

                    return this.$router.go()
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

                    alert("El comentario ha sido borrado con éxito")
                    return this.$router.go()

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

                    alert("El comentario ha sido borrado con éxito")
                    return this.$router.go()

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