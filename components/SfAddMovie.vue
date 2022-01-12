<template>
    <div class="sf-add-movie">

        <v-alert v-if="this.error" border="top" class="text-center" dismissible type="error" color="red" dark> {{this.error}} </v-alert>

        <v-card>
            <v-app-bar>
                <v-icon class="font-weight-black mr-2" size="40"> mdi-plus </v-icon>
                <v-app-bar-title class="font-weight-black"> AÑADIR PELICULA </v-app-bar-title>
            </v-app-bar>

            <v-card-text>
                <v-row>
                    <v-col cols="4">
                        <div class="file-container">
                            <h3 v-if="!url" id="h3">SUBIR PORTADA</h3>
                            <img id="img" v-if="url" :src="url" alt="Image not found" @click="picAgain">
                            <input v-if="!url" type="file" @change="showImg" ref="file" id="imgBtn" class="mt-3" style="background: red; border: 2px solid white; overflow: hidden"/>
                        </div>
                    </v-col>

                    <v-col cols="8">
                        <h6>TITULO DE LA PELICULA</h6>
                        <v-divider class="mb-1" ></v-divider>
                        <v-text-field v-model="title" name="title" placeholder="Ej: 'El resplandor'" outlined />

                        <h6 class="mt-n5">DIRECTOR</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="director" name="director" placeholder="Ej: 'Stanley Kubrick'" outlined />

                        <h6 class="mt-n5">OPINION</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-text-field v-model="description" placeholder="Escriba su opinión sobre la película" outlined height="100px" />

                        <h6 class="mt-n5">PUNTUACIÓN</h6>
                        <v-divider class="mb-1"></v-divider>
                        <v-row>
                            <v-col cols="2">
                                <v-text-field v-model="score" :onkeydown="showScore()" :rules="scoreRules" height="30px" placeholder="0/100" outlined ></v-text-field>
                            </v-col>
                            <v-col cols="10">
                                <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="55">
                                    <strong> {{ Math.ceil(knowledge) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                </v-progress-linear>
                            </v-col>
                        </v-row>
                    </v-col>

                    <v-btn color="rgb(229,9,20)" @click="upload" block> SUBIR PELICULA </v-btn>
                </v-row>
            </v-card-text>
        </v-card>


    </div>
</template>

<script>
export default {
    data() {
        return{
            title: "",
            director: "",
            description: "",
            score: "",
            image: undefined,
            knowledge: "0",
            scoreRules: [
                v => ( v && v >= 0 ) || "El valor mínimo es 0",
                v => ( v && v <= 100 ) || "El valor máximo es 100",
            ],
            error: "",
            url: "",
        }
    },

    methods: {
        async upload(){

            try{

                const config = require('../config')

                const strUserId = localStorage.getItem('userId')

                const userId = JSON.parse(strUserId)
                
                // const formData = new FormData()
                //     formData.enctype = 'multipart/form-data'
                //     formData.append('title', this.title)
                //     formData.append('director', this.director)
                //     formData.append('description', this.description)
                //     formData.append('score', this.score)
                //     formData.append('userId', userId)
                //     formData.append('image', this.url)

                const body = JSON.stringify({
                    title:  this.title,
                    director: this.director,
                    description: this.description,
                    score: this.score,
                    userId: userId,
                    image: this.url
                })

                console.log({body})

                const res = await fetch(config.hostname + 'api/movie/create', {
                    method: 'post',
                    headers: {
                        "Content-Type" : "application/json"
                    },
                    body
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                alert('El post se ha subido con éxito')
                this.$router.push('/misPeliculas')

            }catch(error){
                return console.log(error)
            }
        },

        showImg(){

            const file = this.$refs.file.files[0]
            const reader = new FileReader()
            reader.onloadend = () => {
                console.log(reader.result)
                this.url = reader.result
                console.log("ESTO ES THIS.URL" ,this.url)
            }

            if(file){
                reader.readAsDataURL(file)
            }
            
        },

        picAgain(){
            this.url = ""
        },

        showScore(){
            this.knowledge = this.score
        },


    }

}
</script>


<style scoped>

.file-container{
    background-color: rgb(58, 58, 58);
    height: 405px;
    border: 2px solid white;
    display: flex;
    align-items: center;
    cursor: pointer;
}

#img{
    height: 401px;
    width: 281px;
}

#imgBtn{
    color: rgb(0, 0, 0); opacity: 100%;
    opacity: 0%;
    height: 403px;
    width: 283px;
    cursor: pointer;
}

#h3{
    margin-left: 100px;
    text-align: center;
    color: white;
}

</style>