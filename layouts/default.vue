<template>
    <v-app v-if="visible">
            <SfMenuNav />
        <v-container fluid>
            <v-row>
                <v-col :cols="main"> <Nuxt /> </v-col>
                <v-col :cols="info">
                    <SfPostRecientes />
                    <SfUltimosComentarios />
                    <SfStadisticas />
                </v-col>
            </v-row>
        </v-container>
    </v-app>
</template>

<script>
export default ({
    data() {
        return {
            visible: true,
            main: "",
            info: "",
            value: this.$vuetify.breakpoint.name

        }
    },

    created(){
        this.responsive()
    },
    
    beforeMount(){
        const token = localStorage.getItem("token")
            if(!token){
                this.visible = false
                return this.$router.push("/logIn")
            }
    },

    watch: {
        '$vuetify.breakpoint.name'(value){

            if( value === "xl" || value === "lg" || value === 'md'){
                this.main = "8"
                this.info = "4"
            } else {
                this.main = "12"
                this.info = "12"
            }
        }
    },

    methods: {
        
        responsive(){
            if( this.value === "xl" || this.value === "lg" || this.value === 'md'){
                this.main = "8"
                this.info = "4"
            } else {
                this.main = "12"
                this.info = "12"
            }


        }
    }

})
</script>