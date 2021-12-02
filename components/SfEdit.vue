<template>
    <div class="sf-edit">

        <v-card>
            <v-app-bar>
                <v-icon class="font-weight-black mr-2" size="40"> mdi-movie-edit-outline </v-icon>
                <v-app-bar-title class="font-weight-black"> EDITAR PELICULA </v-app-bar-title>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-col cols="4">
                        <v-file-input v-model="image" v-if="unshow" class="mt-3" height="360px" filled prepend-icon="" style="background: gray; border: 2px solid white" :placeholder="movie.image"/>
                        <v-img v-if="show" :src="post.image" @click="swap()"></v-img>
                    </v-col>

                    <v-col cols="8">
                        <h6>TITULO DE LA PELICULA</h6>
                        <v-divider class="mb-1" ></v-divider>
                        <v-text-field v-model="title" :placeholder="post.title" outlined />

                        <h6 class="mt-n5">DIRECTOR</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="director" :placeholder="post.director" outlined />

                        <h6 class="mt-n5">SINOPSIS</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="description" :placeholder="post.description" outlined height="100px" />

                        <h6 class="mt-n5">PUNTUACIÓN</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-row>
                            <v-col cols="2">
                                <v-text-field v-model="score" :onkeydown="showScore()" :rules="scoreRules" height="30px" outlined ></v-text-field>
                            </v-col>
                            <v-col cols="10">
                                <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="55">
                                    <strong> {{ Math.ceil(knowledge) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                </v-progress-linear>
                            </v-col>
                        </v-row>
                    </v-col>

                    <v-btn color="red" @click="update()" block> EDITAR PELICULA </v-btn>
                </v-row>
            </v-card-text>
        </v-card>

    </div>
</template>

<script>

export default ({
    data(){
        return{
            title: "",
            director: "",
            description: "",
            image: undefined,
            show: true,
            unshow: false,
            score: this.post.score,
            knowledge: "0",
            scoreRules: [
                v => ( v && v >= 0 ) || "El valor mínimo es 0",
                v => ( v && v <= 100 ) || "El valor máximo es 100",
            ],
        }
    },

    props: {
        post: {
            type: Object,
            required: true
        }
    },


    methods: {

        showScore(){
            this.knowledge = this.score
        },

        swap(){
            this.show = false,
            this.unshow = true
        },


        async update(){
            try{

                const formData = new FormData()
                    formData.enctype = 'multipart/form-data'
                    formData.set('title', this.title)
                    formData.set('description', this.description)
                    formData.set('director', this.director)
                    formData.set('image', this.image)
                    formData.set('score', this.score)
                    formData.set('owner', this.movie.user)


                const res = await fetch(`http://localhost:4500/api/movie/update/${this.movie._id}`, {
                    method: 'put',
                    body: formData
                })

                const data = await res.json()
                if(data.error){
                    return alert(data.error)
                }

                alert('El post se ha modificado con éxito')
                this.$router.push(`http://localhost:4500/api/movie/details/${this.post._id}`)

            }catch(error){
                return res.json(error)
            }
        }

    }


})
</script>
