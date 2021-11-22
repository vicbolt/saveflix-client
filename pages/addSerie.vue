<template>
    <div class="sf-add-serie">

        <v-bar-card>
                <v-app-bar shaped>
                    <v-icon class="mr-4" size="35"> mdi-view-grid-plus-outline </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px"> AÑADIR SERIE </v-title>
                    <v-spacer></v-spacer>
                </v-app-bar>
            </v-bar-card>

            <v-card-body>
                <v-row>
                    <v-list-item>
                        <v-list-item-content class="fondo">
                            <v-col cols="4" class="mt-3"> 
                                  <v-file-input label="Subir portada" height="310px" style="label-align: center" filled prepend-icon=""></v-file-input>
                            </v-col>
                            <v-col cols="8">
                                    <v-text-field placeholder="Titulo" outlined class="mb-n4"/>
                                    <v-text-field placeholder="Director" outlined class="mb-n4"/>
                                    <v-text-field height="100px" placeholder="Descripción" outlined class="mb-n4"/>
                                    
                                    <v-progress-linear color="red" v-model="knowledge" height="25">
                                            <strong> {{ Math.ceil(knowledge) }} </strong>
                                    </v-progress-linear>
                            </v-col>
                            <v-btn color="red"> GUARDAR </v-btn>

                        </v-list-item-content>
                    </v-list-item>
                </v-row>
            </v-card-body>



    </div>
</template>

<script>

export default ({
    data() {
        return{
            knowledge: 50
        }
        
    },
})
</script>
