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
                                            <v-img src="https://www.ecartelera.com/carteles/2600/2600/001_m.jpg" />
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "El dia de mañana" </v-list-item-title>
                                            <v-list-item-subtitle class="text-center mb-3" style="color:black"> 99 sobre 100 </v-list-item-subtitle>
                                            <v-btn style="background-color: rgb(229,9,20)" class="verMas">ver mas...</v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                            </v-col>

                           <v-col cols=4>
                                    <v-list-item>
                                        <v-list-item-content class="fondo">
                                            
                                            <v-img src="https://www.ecartelera.com/carteles/2600/2600/001_m.jpg"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "El dia de mañana" </v-list-item-title>
                                            <v-list-item-subtitle class="text-center mb-3" style="color:black"> RATE: 99/100 </v-list-item-subtitle>
                                            <v-btn style="background-color: rgb(229,9,20)" class="verMas">ver mas...</v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                            </v-col>

                           <v-col cols=4>
                                    <v-list-item>
                                        <v-list-item-content class="fondo">
                                            
                                            <v-img src="https://www.ecartelera.com/carteles/2600/2600/001_m.jpg"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "El dia de mañana" </v-list-item-title>
                                            <v-list-item-subtitle class="text-center mb-3" style="color:black"> RATE: 99/100 </v-list-item-subtitle>
                                            <v-btn style="background-color: rgb(229,9,20)">ver mas...</v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                            </v-col>

                     
                            </v-row>
                        </v-card-body>
                    

        </v-card>




    </div>
</template>


<style scoped>

.fondo{
    margin-top: 1em;
    background-color: white;
    color: black;
    padding: 2px;
}
</style>