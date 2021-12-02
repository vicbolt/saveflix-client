<template>
    <div class="sf-chat" v-if="visible">

        <v-row>

            <v-col cols="8">
                <v-row>
                    <v-ul class="list-group"></v-ul>
                </v-row>

                <v-row>
                    <v-col cols="10">
                        <v-text-field placeholder="Escribe un mensaje" class="ml-3"></v-text-field>
                    </v-col>
                    <v-col cols="2">
                        <v-btn color="red" class="mt-4" width="28px"> ENVIAR </v-btn>
                    </v-col>
                </v-row>

            </v-col>




            <!-- CHAT INPUT -->
            <!-- <v-col cols="8">
                <v-card elevation="2" shaped class="mb-5">
                    <v-app-bar>
                        <v-row>
                            <v-col cols="2">
                                <v-img class="ml-n1" id="imagen" src="https://previews.123rf.com/images/jemastock/jemastock1705/jemastock170506999/78062109-cara-de-joven-aislado-sobre-fondo-blanco-expresi%C3%B3n-de-la-cara-hermosa-del-muchacho-ilustraci%C3%B3n-vecto.jpg" width="50px" height="50px" />
                            </v-col>
                            <v-col cols="10">
                                <h3 class="font-weight-black mt-3 ml-n10"> PEDRO </h3>
                            </v-col>
                        </v-row>
                    </v-app-bar>
                    <v-card height="300px">

                    </v-card>
                    <v-row>
                        <v-col cols="10">
                            <v-text-field placeholder="Escribe un mensaje" class="ml-3"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn color="red" class="mt-4" width="28px"> ENVIAR </v-btn>
                        </v-col>
                    </v-row>
                    
                </v-card>
            </v-col> -->

            <!-- END CHAT INPUT -->

            <v-col cols="4">
                <v-card elevation="2" shaped class="mb-5">
                    <v-app-bar>
                        <v-icon> mdi-account-group</v-icon>
                        <v-card-title class="font-weight-black ml-5"> LISTA DE AMIGOS</v-card-title>
                    </v-app-bar>
                <v-row class="mt-2">
                    <v-col cols="4" >
                        <v-img id="imagen" class="ml-n" src="https://previews.123rf.com/images/jemastock/jemastock1705/jemastock170506999/78062109-cara-de-joven-aislado-sobre-fondo-blanco-expresi%C3%B3n-de-la-cara-hermosa-del-muchacho-ilustraci%C3%B3n-vecto.jpg" width="50px" height="50px" />
                    </v-col>
                    <v-col cols="8">
                        <p class="font-weight-black mt-3">PEDRO</p>
                    </v-col>
                </v-row>
                <v-divider class="mt-3 mb-3"></v-divider>

                <!-- PRUEBA -->
                <v-row>
                    <v-col cols="4">
                        <v-img id="imagen" src="https://static.diariofemenino.com/uploads/belleza/82981-CARA.jpg" width="50px" height="50px" />
                    </v-col>
                    <v-col cols="8">
                        <p class="font-weight-black mt-3"> SOFIA </p>
                        
                    </v-col>
                </v-row>
                <!-- PRUEBA -->

                </v-card>
            </v-col>


        </v-row>

    </div>
</template>

<script>
export default ({
    data() {
        return{
            visible: true
        }
    },

    beforeMount(){
        const token = localStorage.getItem('token')

        if(!token){
            this.visible = false
        }
    }
})
</script>

<style scoped>
#imagen{
    border: 2px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 2em;
}
</style>