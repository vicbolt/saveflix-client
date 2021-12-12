<template>
    <div class="sf-explorar" v-if="visible">

    <v-card elevation="2">
        <v-app-bar shaped>
            <v-icon class="mr-4 mb-2" size="35"> mdi-shuriken </v-icon>
            <v-app-bar-title class="font-weight-black mb-2" style="font-size:25px"> EXPLORAR </v-app-bar-title>
        </v-app-bar>

        <v-card>
            <v-row>
                <v-col @click="goTo(title._id)"  cols=4 v-for="title in titles" :key="title.id" >
                    <SfCard id="post" style="cursor: pointer"  :image="title.image" :title="title.title" :score="title.score" />
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
            titles: []
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

                const config = require('/config')

                const res1 = await fetch(config.hostname + `api/movie/explorar`)
                const res2 = await fetch(config.hostname + `api/serial/explorar`)

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

        goTo(movieId){
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