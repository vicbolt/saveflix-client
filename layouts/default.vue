<template>
    <v-app>
            <SfMenuNav />

        <v-container fluid>
            <v-row>
                <v-col cols="8"> <Nuxt /> </v-col>
                <v-col cols="4">
                    <SfPostRecientes />
                    <SfUltimosComentarios />
                    <SfStadisticas />
                </v-col>
            </v-row>
        </v-container>
    </v-app>
</template>