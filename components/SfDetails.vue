<template>
    <div class="sf-details">

        <v-card>
            <v-app-bar shaped style="background: white; color: black">
                    <v-icon class="mr-4" size="35" color="black"> mdi-star </v-icon>
                    <v-app-bar-title class="font-weight-black mt-1" style="font-size:25px"> {{post.title}} </v-app-bar-title>
                    <v-spacer></v-spacer>
                    <v-btn @click="edit(post._id)" class="mr-2"><v-icon color="green">mdi-movie-edit-outline</v-icon></v-btn>
                    <v-btn @click="remove(post._id)"><v-icon color="red">mdi-trash-can-outline</v-icon></v-btn>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3">
                                        <v-img height="420px" style="border: 4px solid white" :src="post.image"></v-img>
                            </v-col>
                            <v-col cols="8">
                                    <h6 class="mb-1">DIRECTOR</h6>
                                    <v-divider class="mb-1" style="width: 160px;"></v-divider>
                                        <h3 class="mb-6"> {{post.director}}</h3>

                                    <h6 class="mb-1">SINOPSIS</h6>
                                    <v-divider class="mb-2" style="width: 560px;"></v-divider>
                                        <p style="max-width:560px; height:180px; text-align: justify; overflow: scroll" class="mb-10"> {{post.description}}</p>
                                    
                                    <h6 class="mb-1 mt-8">PUNTUACIÓN</h6>
                                    <v-divider class="mb-1" style="width: 560px;"></v-divider>
                                    <v-progress-linear color="rgb(229,9,20)" v-model="post.score" height="25" style="width: 560px;">
                                            <strong> {{ Math.ceil(post.score) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                    </v-progress-linear>

                                    <v-row class="mt-4" style="width: 740px;">
                                        <v-col cols="3" >
                                            <v-row >
                                                <v-btn class="ml-2">
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-heart </v-icon>
                                                    <p class="ml-n3 mt-4">60</p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-row>
                                                <v-btn >
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-eye </v-icon>
                                                    <p class="ml-n3 mt-4">88</p>
                                                </v-btn>
                                            </v-row>

                                        </v-col>
                                        <v-col cols="1">
                                            
                                        </v-col>
                                        <v-col cols="3">
                                            <v-row>
                                                <v-btn class="ml-7">
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-share-variant-outline </v-icon>
                                                    <p class="ml-n3 mt-4" style="font-size: 11px"> AÑADIR A MI LISTA </p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                    </v-row>
                            </v-col>
                        </v-list-item-content>
                    </v-list-item>
                </v-row>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>

export default ({
    data(){
        return{
           
        }
    },
    props: {
        post: {
            type: Object,
            required: true
        }
    },


    methods:{
        async remove(postId){
            try{
                const res = await fetch(`http://localhost:4500/api/movie/remove/${postId}`,{
                    method: 'delete',
                    headers: {
                        'Content-type':"application/json"
                    }
                })

                const data = await res.json()
                if(data.error){
                    return alert(data.error)
                }

                alert('El post ha sido borrado con éxito')
                this.$router.push('/misPeliculas')

            }catch(error){
                return res.json(error)
            }
        },

        edit(postId){

            this.$router.push(`/edit/${postId}`)
        }
    }
})

</script>

<style scoped>

.imagen{
    border: 3px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 1em;
}


</style>