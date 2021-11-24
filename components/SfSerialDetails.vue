<template>
    <div class="sf-serial-details">

        <v-bar-card>
            <v-app-bar shaped style="background: white; color: black">
                    <v-icon class="mr-4" size="35" color="black"> mdi-star </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px"> THE OWNERS </v-title>
                    <v-spacer></v-spacer>
                </v-app-bar>
            </v-bar-card>

            <v-card-body>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3"> 
                                  <v-img height="335px" style="border: 4px solid white" src="https://phantom-marca.unidadeditorial.es/ca8ebbfaa8eb053a92f64124cc38a1af/resize/414/f/jpg/assets/multimedia/imagenes/2021/09/02/16305820546830.jpg"></v-img>
                            </v-col>
                            <v-col cols="8" class="mt-n9">
                                    <h6 class="mb-1 mt-10">DIRECTOR</h6>
                                    <v-divider class="mb-3" style="width: 460px;"></v-divider>
                                        <h3 class="mb-5"> Willian Sherkgut</h3>
                                    <h6 class="mb-1">SINOPSIS</h6>
                                    <v-divider class="mb-3" style="width: 460px;"></v-divider>
                                        <p style="width: 460px;"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Delectus, cumque corporis qui sed natus accusantium sit repellendus corrupti, consequatur dolores magni a saepe quibusdam libero odio accusamus, dignissimos obcaecati ratione.</p>
                                    
                                    <h6 class="mb-1 mt-8">PUNTUACIÓN</h6>
                                    <v-divider class="mb-3" style="width: 460px;"></v-divider>
                                    <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="25" style="width: 460px;">
                                            <strong> {{ Math.ceil(knowledge) }} <v-icon class="mr-4 mb-1" size="15" color="white"> mdi-star </v-icon> </strong>
                                    </v-progress-linear>

                                    <v-row class="mt-6">
                                        <v-col cols="3" >
                                            <v-row >
                                                <v-btn class="ml-2">
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-heart </v-icon>
                                                    <p class="ml-n3 mt-4">60</p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-row>
                                                <v-btn >
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-eye </v-icon>
                                                    <p class="ml-n3 mt-4">88</p>
                                                </v-btn>
                                            </v-row>

                                        </v-col>
                                        <v-col cols="1">
                                            
                                        </v-col>
                                        <v-col cols="3">
                                            <v-row>
                                                <v-btn class="ml-7">
                                                    <v-icon class="mr-4" size="20" color="white"> mdi-share-variant-outline </v-icon>
                                                    <p class="ml-n3 mt-4" style="font-size: 11px"> AÑADIR A MI LISTA </p>
                                                </v-btn>
                                            </v-row>
                                        </v-col>
                                    </v-row>
                            </v-col>
                        </v-list-item-content>
                    </v-list-item>
                </v-row>
            </v-card-body>

            <v-divider class="mb-4"></v-divider>
            <v-bar-card>
                <v-app-bar>
                    <v-icon class="mr-4" size="25"> mdi-comment-plus-outline </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:15px"> DEJAR UN COMENTARIO </v-title>
                    <v-spacer></v-spacer>
                </v-app-bar>
            </v-bar-card>

            <v-card-body>
                <v-text-field outlined class="mt-1" placeholder="Escribe tu comentario"></v-text-field>
                <v-btn color="rgb(229,9,20)" class="mt-n6" block> ENVIAR </v-btn>
            </v-card-body>
            <v-divider class="mb-6" ></v-divider>

            <!-- SECCION COMENTARIOS  -->

            <!-- <v-bar-card>
                <v-app-bar>
                    <v-icon class="mr-4" size="35">  </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px">COMENTARIOS </v-title>
                    <v-spacer></v-spacer>
                </v-app-bar>
            </v-bar-card> -->

            <v-card-body>
                <v-row>
                    <v-col cols="2">
                        <v-img class="imagen" src="https://phantom-marca.unidadeditorial.es/ca8ebbfaa8eb053a92f64124cc38a1af/resize/414/f/jpg/assets/multimedia/imagenes/2021/09/02/16305820546830.jpg" width="70px" height="70px" />
                    </v-col>
                    <v-col cols="10">
                        <p class="font-weight-black mt-2 ml-n7" style="background:white; color:black"> #USERNAME </p>
                        <p class="ml-n7 mt-n3"> #COMENTARIO </p>
                    </v-col>
                </v-row>
                <v-divider class="mt-4 mb-4"></v-divider>
            </v-card-body>




    </div>
</template>

<script>

export default ({
    data() {
      return {
          knowledge: "40",
      }  
    },
})
</script>

<style scoped>

.imagen{
    border: 3px solid rgb(229,9,20);
    border-radius: 100%;
    margin-left: 1em;
}


</style>