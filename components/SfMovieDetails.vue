<template>
    <div class="sf-serial-details">

        <v-bar-card>
            <v-app-bar shaped style="background: white; color: black">
                    <v-icon class="mr-4" size="35" color="black"> mdi-star </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px"> #TITULO DE LA PELICULA </v-title>
                    <v-spacer></v-spacer>
                </v-app-bar>
            </v-bar-card>

            <v-card-body>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3"> 
                                  <v-img style="border: 4px solid white" src="https://heraldodemexico.com.mx/u/fotografias/m/2021/8/4/f768x1-404129_404256_142.jpeg"></v-img>
                            </v-col>
                            <v-col cols="8">
                                    <h3 class="mb-4 mt-n12"> #DIRECTOR</h3>
                                    <v-divider class="mb-4"></v-divider>
                                    <p class="mb-5"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Delectus, cumque corporis qui sed natus accusantium sit repellendus corrupti, consequatur dolores magni a saepe quibusdam libero odio accusamus, dignissimos obcaecati ratione.</p>

                                    <h6 class="mb-1">PUNTUACIÓN</h6>
                                    <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="25">
                                            <strong> {{ Math.ceil(knowledge) }} </strong>
                                    </v-progress-linear>
                            </v-col>
                        </v-list-item-content>
                    </v-list-item>
                </v-row>
            </v-card-body>

            <v-divider class="mb-4"></v-divider>
            <v-bar-card>
                <v-app-bar>
                    <v-icon class="mr-4" size="35"> mdi-comment-plus-outline </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px"> DEJAR UN COMENTARIO </v-title>
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
                        <v-img class="imagen" src="https://previews.123rf.com/images/jemastock/jemastock1705/jemastock170506999/78062109-cara-de-joven-aislado-sobre-fondo-blanco-expresi%C3%B3n-de-la-cara-hermosa-del-muchacho-ilustraci%C3%B3n-vecto.jpg" width="70px" height="70px" />
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