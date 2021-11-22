<template>
    <div class="sf-mi-ranking">

        <v-row>
            <v-col cols=6>
            <v-card elevation="2">
            <v-bar-card>
                <v-app-bar shaped class="d-flex justify-center mb-6" style="background-color:rgb(229,9,20)">
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                    <v-title class="font-weight-black" style="font-size: 25px"> TOP PELICULAS </v-title>
                </v-app-bar>
            </v-bar-card>
                <v-card-body>
                    <v-list>
                        <div class="container-movies mb-5" style="background-color: black">
                            <v-row>
                                <v-col cols="2">
                                    <h1 class="text-center mt-3" style="color:white; background-color:rgb(229,9,20);">1#</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" src="https://es.web.img2.acsta.net/pictures/20/11/10/10/03/4913863.jpg" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="7">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px"> EL FEO EL BUENO Y EL MALO </p> 
                                </v-col> 
                                <div>
                                <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>

                        


                        <div class="container-movies" style="background-color: black">
                            <v-row>
                                <v-col cols="2">
                                    <h1 class="text-center mt-3" style="color:white; background-color:rgb(229,9,20);">2#</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" src="https://es.web.img2.acsta.net/pictures/20/11/10/10/03/4913863.jpg" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="7">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px"> EL FEO EL BUENO Y EL MALO </p> 
                                </v-col> 
                                <v-divider></v-divider>
                                <div>
                                
                                </div>
                            </v-row>
                        </div>
                    </v-list>
                </v-card-body>         
            </v-card>
            </v-col>

            <v-col cols=6>
            <v-card elevation="2">
            <v-bar-card>
                <v-app-bar shaped class="d-flex justify-center mb-6" style="font-size: 25px; background-color:rgb(229,9,20)" >
                    <v-icon class="mr-4" size="30"> mdi-star </v-icon>
                    <v-title class="font-weight-black"> TOP SERIES </v-title>
                </v-app-bar>
            </v-bar-card>
                <v-card-body>
                    <v-list>
                        <div class="container-movies mb-5" style="background-color: black">
                            <v-row>
                                <v-col cols="2">
                                    <h1 class="text-center mt-3" style="color:white; background-color:rgb(229,9,20);">1#</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" src="https://es.web.img2.acsta.net/pictures/20/11/10/10/03/4913863.jpg" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="7">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px"> EL FEO EL BUENO Y EL MALO </p> 
                                </v-col> 
                                <div>
                                <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                        <div class="container-movies" style="background-color: black">
                            <v-row>
                                <v-col cols="2">
                                    <h1 class="text-center mt-3" style="color:white; background-color:rgb(229,9,20);">2#</h1>
                                </v-col>
                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" src="https://es.web.img2.acsta.net/pictures/20/11/10/10/03/4913863.jpg" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="7">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px"> EL FEO EL BUENO Y EL MALO </p> 
                                </v-col> 
                                <v-divider></v-divider>
                                <div>
                                
                                </div>
                            </v-row>
                        </div>
                    </v-list>
                </v-card-body>       

            </v-card>
            </v-col>
        </v-row>

    </div>
</template>

<style scoped>

#fondo{
    background-color: white;
    color: black;
    padding: 3px;
    border-radius: 2%;
}
</style>