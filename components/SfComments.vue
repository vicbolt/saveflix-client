<template>
    <div class="sf-comments">
        <v-divider class="mb-4"></v-divider>
            <v-card>
                <v-app-bar>
                    <v-icon class="mr-4" size="25"> mdi-comment-plus-outline </v-icon>
                    <v-app-bar-title class="font-weight-black mt-1" style="font-size:15px"> DEJAR UN COMENTARIO </v-app-bar-title>
                    <v-spacer></v-spacer>
                </v-app-bar>

                <v-card-text>
                    <v-text-field v-model="message" outlined class="mt-1" placeholder="Escribe tu comentario"></v-text-field>
                    <v-btn color="rgb(229,9,20)" class="mt-n6" block @click="sendComment(post._id)"> ENVIAR </v-btn>
                </v-card-text>
                <v-divider class="mb-6" ></v-divider>

                <v-card-text>
                    <v-row v-for="comment in comments" :key="comment._id">
                        <v-col cols="2">
                            <v-img class="imagen" :src="comment.user.avatar" width="70px" height="70px" />
                        </v-col>
                        <v-col cols="10">
                            <p class="font-weight-black mt-2 ml-n7" style="background:white; color:black"> {{comment.user.username}} </p>
                            <p class="ml-n7 mt-n3"> {{comment.message}} </p>
                        </v-col>
                    </v-row>
                    <v-divider class="mt-4 mb-4"></v-divider>
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
        }
    },

    props: {
        post: {
            type: Object,
            required: true
        }
    },

    async beforeMount(){
        await this.getComments()
    },

    methods: {
        async sendComment(postId){
            try{
                const config = require('../config')

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){

                    const userId = JSON.parse(localStorage.getItem('userId'))

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
                        return alert(data.error)
                    }

                    alert('El comentario ha sido enviado')

                    this.$router.push(`/details/${this.postId}`)

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
                        return alert(data1.error)
                    }

                    alert('El comentario ha sido enviado')

                    this.$router.go(`/details/${this.postId}`)
                }

            } catch(error){
                return console.log(error)
            }
        },

        async getComments(){

            try{
                const config = require('../config')

                const postId = this.post._id

                const res = await fetch(config.hostname + `api/movie/getOne/${postId}`)
                const data = await res.json()

                if(data.movie !== null){
                    const postId = this.post._id

                    const res1 = await fetch(config.hostname + `api/movieComment/getComments/${postId}`)

                    const data1 = await res1.json()
                    if(data1.error){
                        return alert(data1.error)
                    }

                    for(const comment of data1.comments){
                        this.comments.push(comment)
                    }
                } else {

                    const postId = this.post._id

                    const res2 = await fetch(config.hostname + `api/serialComment/getComments/${postId}`)

                    const data2 = await res2.json()
                    if(data2.error){
                        return alert(data2.error)
                    }

                    for(const comment of data2.comments){
                        this.comments.push(comment)
                    }
                }

            } catch(error){
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