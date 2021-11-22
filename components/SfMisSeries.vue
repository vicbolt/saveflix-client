<template>
    <div class="sf-mis-series">

 <v-card elevation="2">
            <v-bar-card>
                <v-app-bar shaped>
                    <v-icon class="mr-4" size="35"> mdi-television-classic </v-icon>
                    <v-title class="font-weight-black mt-1" style="font-size:25px"> MIS SERIES </v-title>
                    <v-spacer></v-spacer>
                    <v-btn color="green">AÑADIR SERIE</v-btn>
                </v-app-bar>
            </v-bar-card>

                        <v-card-body>
                            <v-row>
                                <v-col cols=4>
                                    <v-list-item>
                                        <v-list-item-content class="fondo">
                                            <v-img height="280px" src="https://media.revistagq.com/photos/5ebbbff55663621a3c7fc9fe/master/pass/you.jpg" />
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "You" </v-list-item-title>
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
                                            
                                            <v-img height="280px" src="https://i.pinimg.com/736x/2e/b5/b3/2eb5b318f8758c202e84802bd8bde2d0.jpg"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "The 100" </v-list-item-title>
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
                                            
                                            <v-img height="280px" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSUcyHnSNtQKXhN_sxhS0KFZkMQTCwjlILV3g&usqp=CAU"/>
                                            <v-list-item-title class="text-center mt-2 font-weight-black"> "Black Mirror" </v-list-item-title>
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
          knowledge: "35",
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