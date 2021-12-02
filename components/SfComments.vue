<template>
    <div class="sf-comments">
        <v-divider class="mb-4"></v-divider>
            <v-card>
                <v-app-bar>
                    <v-icon class="mr-4" size="25"> mdi-comment-plus-outline </v-icon>
                    <v-app-bar-title class="font-weight-black mt-1" style="font-size:15px"> DEJAR UN COMENTARIO </v-app-bar-title>
                    <v-spacer></v-spacer>
                </v-app-bar>

                <v-card-text>
                    <v-text-field outlined class="mt-1" placeholder="Escribe tu comentario"></v-text-field>
                    <v-btn color="rgb(229,9,20)" class="mt-n6" block> ENVIAR </v-btn>
                </v-card-text>
                <v-divider class="mb-6" ></v-divider>

                <v-card-text>
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
                </v-card-text>
            </v-card>

    </div>
</template>