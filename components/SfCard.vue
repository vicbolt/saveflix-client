<template>
    <div class="sf-card">

        <v-list-item>
            <v-list-item-content class="fondo">
                <v-img height="280px"  :src="image">
                     <v-btn v-if="tag === 'serial'" style="font-size: 9px" width="auto" height="auto"  color="rgb(229,9,20)"> SERIE</v-btn>
                     <v-btn v-if="tag === 'movie'" style="font-size: 9px" width="auto" height="auto"  color="rgb(229,9,20)"> PELICULA</v-btn>
                </v-img>
                <v-list-item-title class="text-center mt-2 font-weight-black"> {{title}} </v-list-item-title>
                    <v-progress-linear color="rgb(229,9,20)" v-model="score" height="25">
                        <strong> {{ Math.ceil(score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                    </v-progress-linear>
                    <v-btn class="boton">ver mas...</v-btn>
            </v-list-item-content>
        </v-list-item>

    </div>
</template>


<script>

export default {


    props: {
        image: {
            type: String,
            required: true,
        },
        title: {
            type: String,
            required: true,
        },
        score: {
            type: Number,
            required: true,
        },
        tag: {
            type: String,
            required: true
        }
    },

}
</script>


<style scoped>
.fondo{
    margin-top: 1em;
    background-color: white;
    color: black;
    padding: 2px;
}

</style>
