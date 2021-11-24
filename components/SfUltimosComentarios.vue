<template>
    <div class="sf-ultimos-comentarios" v-if="visible">

                <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-icon> mdi-comment-multiple-outline</v-icon>
                <v-app-bar-title class="font-weight-black ml-5"> <strong> ÚLTIMOS COMENTARIOS </strong> </v-app-bar-title>
            </v-app-bar>

            <v-list>
                <div class="rs-recent-comments-container mt-4">

                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" src="https://previews.123rf.com/images/jemastock/jemastock1705/jemastock170506999/78062109-cara-de-joven-aislado-sobre-fondo-blanco-expresi%C3%B3n-de-la-cara-hermosa-del-muchacho-ilustraci%C3%B3n-vecto.jpg" width="70px" height="70px" />
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"> <strong>  USERNAME </strong> ha comentado en <strong> TITULO_PELICULA </strong> </p>
                    </v-col>
                </v-row>
                <v-divider class="mt-4 mb-4"></v-divider>

                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" src="https://static.diariofemenino.com/uploads/belleza/82981-CARA.jpg" width="70px" height="70px" />
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"><strong> USERNAME </strong> ha comentado en <strong> TITULO_PELICULA </strong></p>
                    </v-col>
                </v-row>
                <v-divider class="mt-4 mb-4"></v-divider>

                </div>
            </v-list>

        </v-card>

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
.imagen{
    border: 3px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 2em;
}
</style>