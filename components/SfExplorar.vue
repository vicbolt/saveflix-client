<template>
    <div class="sf-explorar" v-if="visible">

    <v-card elevation="2">
        <v-app-bar shaped>
            <v-icon class="mr-4 mb-2" size="35"> mdi-shuriken </v-icon>
            <v-app-bar-title class="font-weight-black mb-2" style="font-size:25px"> EXPLORAR </v-app-bar-title>
        </v-app-bar>

        <v-card>
            <v-row>
                <v-col cols=4 v-for="movie in movies" :key="movie.id">
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-img height="280px"  :src="movie.image"/>
                            <v-list-item-title class="text-center mt-2 font-weight-black"> {{movie.title}} </v-list-item-title>
                                <v-progress-linear color="rgb(229,9,20)" v-model="movie.score" height="25">
                                    <strong> {{ Math.ceil(movie.score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                </v-progress-linear>
                                <v-btn class="boton">ver mas...</v-btn>
                        </v-list-item-content>
                    </v-list-item>
                </v-col>

                <v-col cols=4  v-for="serial in serials" :key="serial.id">
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-img height="280px" :src="serial.image" />
                            <v-list-item-title class="text-center mt-2 font-weight-black"> {{serial.title}} </v-list-item-title>
                                <v-progress-linear color="rgb(229,9,20)" v-model="serial.score" height="25">
                                    <strong> {{ Math.ceil(serial.score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                </v-progress-linear>
                                    <v-btn class="boton">ver mas...</v-btn>
                        </v-list-item-content>
                    </v-list-item>
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
            movies: [],
            serials: []
        }
    },

    async beforeMount(){
        // const token = localStorage.getItem('token')

        // if(!token){
        //     this.visible = false
        // }

        await this.loadMoviesAndSerials()
    },

    methods: {
        async loadMoviesAndSerials(){
            try{
                const res1 = await fetch('http://localhost:4500/api/movie/getAll')
                const res2 = await fetch('http://localhost:4500/api/serial/getAll')

                const data1 = await res1.json()
                if(data1.error){
                    return alert(data1.error) 
                }

                const data2 = await res2.json()
                if(data2.error){
                    return alert(data2.error) 
                }

                this.movies = []
                this.serials = []
                for(const movie of data1.movies){
                    this.movies.push(movie)
                }

                for(const serial of data2.serials){
                    this.serials.push(serial)
                }

 
            }catch(error){
                return res1.json(error)
            }
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
</style>