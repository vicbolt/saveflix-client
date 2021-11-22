<template>
    <div class="sf-stadisticas">

        <v-card elevation="2" shaped>
            <v-app-bar>
                <v-icon> mdi-go-kart-track</v-icon>
                <v-title class="font-weight-black ml-5"> ESTADÍSTICAS</v-title>
            </v-app-bar>

            <v-list>
                <v-list-item>
                    <v-list-icon>
                        <v-icon id="icon" class="mr-5">mdi-movie-open-star-outline</v-icon>
                    </v-list-icon>
                    <v-list-content class="font-weight-black"> #n PELICULAS SUBIDAS </v-list-content>
                </v-list-item>
            
                <v-list-item>
                    <v-list-icon>
                        <v-icon id="icon" class="mr-5"> mdi-television-classic</v-icon>
                    </v-list-icon>
                    <v-list-content class="font-weight-black"> #n SERIES SUBIDAS </v-list-content>
                </v-list-item>

            </v-list>

        </v-card>


    </div>
</template>

<style scoped>

#icon{
color:rgb(229,9,20)
}
</style>