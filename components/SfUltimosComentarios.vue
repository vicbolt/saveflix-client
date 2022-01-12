<template>
    <div class="sf-ultimos-comentarios">

        <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-icon> mdi-comment-multiple-outline</v-icon>
                <v-app-bar-title class="font-weight-black ml-5"> <strong> ÚLTIMOS COMENTARIOS </strong> </v-app-bar-title>
            </v-app-bar>

            <v-list>
                <div v-for="comment in comments" :key="comment.id" @click="goTo(comment.post._id)" style="cursor:pointer" class="mt-4">
                    <v-row>
                        <v-col cols="4">
                            <v-img class="imagen" :src="comment.user.avatar" width="70px" height="70px" />
                        </v-col>
                        <v-col cols="8" class="ml-n5">
                            <p class="mt-2"> <strong>  {{comment.user.username}} </strong> ha comentado en <strong> {{comment.post.title}} </strong> </p>
                        </v-col>
                    </v-row>
                    <v-divider class="mt-4 mb-4"></v-divider>
                </div>
            </v-list>
        </v-card>

    </div>
</template>

<script>
export default ({
    data() {
        return{
            comments: []
        }
    },

    async beforeMount(){
        
        await this.ultimosComentarios()
    },

    methods: {

       async ultimosComentarios(){
            try{

                
                const config = require('../config')
                const res = await fetch(config.hostname + 'api/movieComment/latestComments')

                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }


                const res2 = await fetch(config.hostname + 'api/serialComment/latestComments')
                const data2 = await res2.json()
                if(data2.error){
                    console.log(data2.error)
                }

                const allComments = data.comments.concat(data2.comments)
                
            
                for(const comment of allComments){
                    this.comments.push(comment)
                }

                
            }catch(error){
                return console.log(error)
            }
        },

        goTo(postId){
            
            localStorage.removeItem("postId")
            localStorage.setItem("postId", postId)
            this.$router.go(`/details/${postId}`)
        }
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