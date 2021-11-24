<template>
    <div class="sf-post-recientes" v-if="visible">

        <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-icon> mdi-fire</v-icon>
                <v-app-bar-title class="font-weight-black ml-5"> POST RECIENTES </v-app-bar-title>
            </v-app-bar>

            <v-list>
                <div class="rs-recent-upload-container mt-4">
                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" src="https://es.web.img2.acsta.net/pictures/20/11/10/10/03/4913863.jpg" width="70px" height="70px" />
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"> <strong> USERNAME </strong> ha subido <strong> TITULO_PELICULA </strong></p>
                    </v-col> 
                </v-row>
                <v-divider class="mt-4 mb-4"></v-divider>
                

                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" src="https://img.vixdata.io/pd/jpg-large/es/sites/default/files/e/eerie-pelicula-terror-filipinas-2019-netflix-portada-056203.jpg" width="70px" height="70px"/>
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"> <strong> USERNAME </strong> ha subido <strong> TITULO_PELICULA </strong></p>
                    </v-col>
                </v-row>
                <v-divider class="mt-4 mb-4"></v-divider>

                <v-row>
                    <v-col cols="4">
                        <v-img class="imagen" src="https://www.ecartelera.com/carteles/2600/2600/001_m.jpg" width="70px" height="70px"/>
                    </v-col>
                    <v-col cols="8" class="ml-n5">
                        <p class="mt-2"> <strong> USERNAME </strong> ha subido <strong> TITULO_PELICULA </strong> </p>
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
    margin-left: 2em;
  border: 6px solid;
  border-image-slice: 1;
  border-width: 3px;
    border-image-source: linear-gradient(to left, #aa1515, #711f7c);
}

</style>