<template>
    <v-app v-if="visible">
            <SfMenuNav />

        <v-container fluid>
            <v-row>
                <v-col cols="8"> <Nuxt /> </v-col>
                <v-col cols="4">
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
            visible: true
        }
    },
    
    beforeMount(){
        const token = localStorage.getItem("token")
            if(!token){
                this.visible = false
                return this.$router.push("/logIn")
            }
    }

})
</script>