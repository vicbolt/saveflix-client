<template>
    <div class="sf-stadisticas">

        <v-card elevation="2" shaped v-if="visible">
            <v-app-bar>
                <v-icon> mdi-go-kart-track</v-icon>
                <v-app-bar-title class="font-weight-black ml-5"> ESTADÍSTICAS GLOBALES</v-app-bar-title>
            </v-app-bar>

            <v-list>
                <v-list-item>
                    <v-list-item-icon>
                        <v-icon id="icon" class="mr-5">mdi-movie-open-star-outline</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content class="font-weight-black"> {{movies}} PELICULAS SUBIDAS  </v-list-item-content>
                </v-list-item>
            
                <v-list-item>
                    <v-list-item-icon>
                        <v-icon id="icon" class="mr-5"> mdi-television-classic</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content class="font-weight-black"> {{serials}} SERIES SUBIDAS  </v-list-item-content>
                </v-list-item>

            </v-list>

        </v-card>


    </div>
</template>

<script>

export default ({
    data() {
        return{
            visible: true,
            serials: "",
            movies: ""
        }
    },

    async beforeMount(){
        const token = localStorage.getItem('token')

        if(!token){
            this.visible = false
        }

        await this.stats()

    },

    methods: {
        
        async stats(){

            try{
                //stats de movies

                const config = require('../config')

                const res1 = await fetch(config.hostname + 'api/movie/stats')
                const moviesData = await res1.json()

                if(moviesData.error){
                    return alert(moviesData.error)
                }
                this.movies = moviesData.movies


                //stats de serials
                const res2 = await fetch(config.hostname + 'api/serial/stats')

                const serialsData = await res2.json()

                if(serialsData.error){
                    return alert(serialsData.error)
                }
                this.serials = serialsData.serials


            }catch(error){
                return console.log(error)
            }


        }

    }
})
</script>


<style scoped>

#icon{
color:rgb(229,9,20)
}
</style>