<template>
    <v-app v-if="visible">
        
        <SfMenuNav />

        <v-container fluid>
           
            <v-row>
                <v-col cols="10 offset-1" class="fondo"> <Nuxt /> </v-col>
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
