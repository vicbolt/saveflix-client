<template>
    <div class="sf-mis-peliculas">

 <v-card elevation="2">
            <v-bar-card>
                <v-app-bar shaped>
                    <v-icon class="mr-4" size="35"> mdi-movie-open-star-outline </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px"> MIS PELICULAS </v-title>
                    <v-spacer></v-spacer>
                    <v-btn color="green">AÑADIR PELICULA</v-btn>
                </v-app-bar>
            </v-bar-card>

                        <v-card-body>
                            <v-row>
                                <v-col cols=4>
                                    <v-list-item>
                                        <v-list-item-content class="fondo">
                                            <v-img height="280px" src="https://www.ecartelera.com/carteles/2600/2600/001_m.jpg" />
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "El dia de mañana" </v-list-item-title>
                                                    <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="25">
                                                        <strong> {{ Math.ceil(knowledge) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                                </v-progress-linear>
                                            <v-btn class="boton">ver mas...</v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                            </v-col>

                           <v-col cols=4>
                                    <v-list-item>
                                        <v-list-item-content class="fondo">
                                            
                                            <v-img height="280px" src="https://www.instyle.es/medio/2020/04/16/perdi-mi-cuerpo-pelicula-netflix-1_fedca154_900x1243.jpg"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "I lost my body" </v-list-item-title>
                                                <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="25">
                                                        <strong> {{ Math.ceil(knowledge) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                                </v-progress-linear>
                                            <v-btn class="boton">ver mas...</v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                            </v-col>

                           <v-col cols=4>
                                    <v-list-item>
                                        <v-list-item-content class="fondo">
                                            <v-img height="280px" src="https://tendenciasdebodas.com/wp-content/uploads/2018/11/tendenciasdebodas-que-ver-series-netflix-thesinner.jpg"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "The sinner" </v-list-item-title>
                                                <v-progress-linear color="rgb(229,9,20)" v-model="knowledge" height="25">
                                                        <strong> {{ Math.ceil(knowledge) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                                </v-progress-linear>
                                            <v-btn class="boton">ver mas...</v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                            </v-col>
                            </v-row>
                        </v-card-body>
        </v-card>
    </div>
</template>

<script>

export default ({
    data() {
      return {
          knowledge: "97",
      }  
    },
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
</style>