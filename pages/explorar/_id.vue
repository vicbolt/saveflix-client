<template>
    <div class="sf-explorar" v-if="visible">

        <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>
        
        <v-card>
            <v-app-bar height="auto" class="pa-4" shaped>
                <v-row>
                    <v-col :cols="explorar">
                        <v-row>
                            <v-icon class="mr-4 mt-3" size="35"> mdi-shuriken </v-icon>
                            <v-app-bar-title class="font-weight-black mt-3" :style="tamañoFuente"> EXPLORAR </v-app-bar-title>
                        </v-row>
                    </v-col>
                    <v-col :cols="buscador">
                        <v-row>
                            <v-text-field v-model="search" placeholder="Buscar por título" type="text" class="mt-3" v-on:keyup.enter="buscar"></v-text-field>
                            <v-btn class="ml-2 mt-2" color="red" height="40px" min-width="25px" @click="buscar"> <v-icon size="20px">mdi-movie-search-outline</v-icon> </v-btn>
                        </v-row>
                    </v-col>
                </v-row>
            </v-app-bar>


                <div v-if="loading" style="text-align:center">
                    <img height="120em" src="@/static/load.gif" class="mb-6" />  
                </div>
            <v-card>
                <v-row>
                    <v-col @click="goTo(title._id)" :cols="cards" v-for="title in titles" :key="title.id">
                        <SfCard id="post" style="cursor: pointer;" :image="title.image" :title="title.title" :score="title.score" :tag="title.post" />
                    </v-col>
                            
                </v-row>
            </v-card>

        </v-card>
    </div>
</template>

<script>

export default ({
    data(){
        return {
            visible: true,
            titles: [],
            search: "",
            error: "",
            cards: "",
            tamañoFuente: "",
            explorar: "",
            buscador: "",
            loading: true,
            value: this.$vuetify.breakpoint.name
            
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

        await this.loadMoviesAndSerials()
        this.loading = false
    },

    watch: {
        '$vuetify.breakpoint.name'(value){

            if( value === "xl" || value === "lg"){
                this.cards = "4"
                this.explorar = "7"
                this.buscador = "5"
                this.tamañoFuente = "font-size:25px"
            }
            
            if( value === "md" || value === "sm"){
                this.cards = "6"
                this.explorar = "7"
                this.buscador = "5"
                this.tamañoFuente = "font-size:25px"
            }
            
            if(value === "xs") {
                
                this.cards = "12"
                this.explorar = "12"
                this.buscador = "12"
                this.tamañoFuente = "font-size:20px"
            }
        }
    },

    methods: {

        responsive(){

            if( this.value === "xl" || this.value === "lg"){
                this.cards = "4"
                this.explorar = "7"
                this.buscador = "5"
                this.tamañoFuente = "font-size:25px"
            }
            
            if( this.value === "md" || this.value === "sm"){
                this.cards = "6"
                this.explorar = "7"
                this.buscador = "5"
                this.tamañoFuente = "font-size:25px"
            }
            
            if(this.value === "xs") {
                
                this.cards = "12"
                this.explorar = "12"
                this.buscador = "12"
                this.tamañoFuente = "font-size:20px"
            }

        },

        async loadMoviesAndSerials(){
            try{
                const config = require('/config')

                const res1 = await fetch(config.hostname + `api/movie/explorar`)
                const res2 = await fetch(config.hostname + `api/serial/explorar`)

                const data1 = await res1.json()
                if(data1.error){
                    return console.log(data1.error) 
                }

                const data2 = await res2.json()
                if(data2.error){
                    return console.log(data2.error) 
                }

                for(const title of data1.movies){
                    this.titles.push(title)
                }

                for(const title of data2.serials){
                    this.titles.push(title)
                }
                const total = data1.movies.length + data2.serials.length //suma de peliculas y series

                const verify = []

                const aux = []

                while(aux.length < total){
                    let n = Math.floor(Math.random()*total)
                    if(!verify.includes(n)){
                        aux.push(this.titles[n])
                        verify.push(n)
                    }
                }

                this.titles = aux

 
            }catch(error){
                return res1.json(error)
            }
        },

        async buscar(){
            try{
                const config = require('/config')

                const title1 = this.search
                const title = title1.replace(/ /g, "").toUpperCase()

                //BUSCAR PELICULA
                const res = await fetch(config.hostname + `api/movie/search/${title}`)
                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                localStorage.setItem("postId", data)
                this.$router.push(`/details/${data}`)

            }catch(error){
                return console.log(error)
            }
        },

        goTo(movieId){
            localStorage.setItem("postId", movieId)
            this.$router.push(`/details/${movieId}`)
        }
    }
})
</script>

<style scoped>

.fondo{
    margin-top: 1em;
    background-color: white;
    color: black;
    padding: 2px;
}

.boton{
    background-color: rgb(18, 18, 18);
    border: 2px solid rgb(229,9,20);
}

#post:hover{
    zoom: 101%;
}
</style>