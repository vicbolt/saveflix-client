<template>
    <v-app>
        <v-bar>
            <SfMenuNav />
        </v-bar>
        <v-container fluid>
           
            <v-row>
                <v-col cols="10 offset-1" class="fondo"> <Nuxt /> </v-col>
            </v-row>
            
        </v-container>
        
    </v-app>
</template>

<style scoped>

.v-application{
    background-image: url("https://wallpapercave.com/wp/wp2757874.gif");

}

</style>