<template>
    <v-app>
            <SfMenuNav />

        <v-container fluid>
            <v-row>
                <v-col :cols="peliculas"> <Nuxt /> </v-col>
                <v-col :cols="peliculasP">
                    <SfMoviesP/>
                </v-col>
            </v-row>
        </v-container>
    </v-app>
</template>

<script>

export default({
    data() {
        return{
            peliculas: "",
            peliculasP: "",
            value: this.$vuetify.breakpoint.name
        }
    },

    created(){
        this.responsive()
    },

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)

            if(value === "xl" || value === "lg"){
                this.peliculas = "8"
                this.peliculasP = "4"
            } else{
                this.peliculas = "12"
                this.peliculasP = "12"
            }
        }
    },

    methods: {
        responsive(){

            if(this.value === "xl" || this.value === "lg"){
                this.peliculas = "8"
                this.peliculasP = "4"
            } else{
                this.peliculas = "12"
                this.peliculasP = "12"
            }
        }
    }
})
</script>
