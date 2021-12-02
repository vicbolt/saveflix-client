<template>
    <div class="sf-post-recientes" v-if="visible">

        <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-icon> mdi-fire</v-icon>
                <v-app-bar-title class="font-weight-black ml-5"> POST RECIENTES </v-app-bar-title>
            </v-app-bar>

            <v-list>
                <div v-for="title in titles" :key="title.id" class="rs-recent-upload-container mt-4">
                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" :src="title.image" width="70px" height="70px" />
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"> <strong> {{title.user}} </strong> ha subido <strong> {{title.title}} </strong></p>
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
            visible: true,
            titles: []
        }
    },

    async beforeMount(){

        await this.postRecientes()
    },

    methods: {

        async postRecientes(){
        try{
                const res1 = await fetch('http://localhost:4500/api/movie/postRecientes')
                const res2 = await fetch('http://localhost:4500/api/serial/postRecientes')

                const data1 = await res1.json()
                if(data1.error){
                    return alert(data1.error) 
                }

                const data2 = await res2.json()
                if(data2.error){
                    return alert(data2.error) 
                }

                for(const title of data1.movies){
                    this.titles.push(title)
                }

                for(const title of data2.serials){
                    this.titles.push(title)
                }

 
            } catch(error){
                return res1.json(error)
            }
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