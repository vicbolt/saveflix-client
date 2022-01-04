<template>
    <div class="sf-post-recientes">

        <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-icon> mdi-fire</v-icon>
                <v-app-bar-title class="font-weight-black ml-5"> POST RECIENTES </v-app-bar-title>
            </v-app-bar>

            <v-list>
                <div v-for="post in posts" :key="post.id" @click="goTo(post._id)" class="rs-recent-upload-container mt-4" style="cursor:pointer">
                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" :src="post.image" width="70px" height="70px" />
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"> <strong> {{post.userId.username}} </strong> ha subido <strong> {{post.title}} </strong></p>
                    </v-col> 
                </v-row>
                <v-divider class="mt-4 mb-4"></v-divider>
                </div>
            </v-list>

        </v-card>
    </div>
</template>

<script>

export default {
    data() {
        return{
            posts: []
        }
    },

    async beforeMount(){

        await this.postRecientes()
    },

    methods: {

        async postRecientes(){
        try{
            const config = require('../config')

                const res1 = await fetch(config.hostname + 'api/movie/postRecientes')
                const res2 = await fetch(config.hostname + 'api/serial/postRecientes')

                const data1 = await res1.json()
                if(data1.error){
                    return alert(data1.error) 
                }

                const data2 = await res2.json()
                if(data2.error){
                    return alert(data2.error) 
                }

                const movies = data1.movies
                const serials = data2.serials

                const posts = movies.concat(serials)  //.sort({createdAt: 'desc'})


                for(const post of posts){
                    this.posts.push(post)
                }

                // console.log(movies)
                // console.log(serials)
                // console.log(posts)

            } catch(error){
                return res1.json(error)
            }
        },

    goTo(postId){
        console.log(postId)
        this.$router.push(`/details/${postId}`)
    }
    },


}
</script>


<style scoped>

.imagen{
    margin-left: 2em;
  border: 6px solid;
  border-image-slice: 1;
  border-width: 3px;
    border-image-source: linear-gradient(to left, #aa1515, #711f7c);
}

</style>