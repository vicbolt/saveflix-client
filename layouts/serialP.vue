<template>
    <v-app>
            <SfMenuNav />

        <v-container fluid>
            <v-row>
                <v-col :cols="series"> <Nuxt /> </v-col>
                <v-col :cols="seriesP">
                    <SfSerialP/>
                </v-col>
            </v-row>
        </v-container>
    </v-app>
</template>

<script>

export default({
    data() {
        return{
            series: "",
            seriesP: "",
            value: this.$vuetify.breakpoint.name
        }
    },


    created(){
        this.responsive()
    },

    watch: {
        '$vuetify.breakpoint.name'(value){

            if(value === "xl" || value === "lg"){
                this.series = "8"
                this.seriesP = "4"

            } else{
                this.series = "12"
                this.seriesP = "12"
            }
        }
    },

    methods: {
        responsive(){
            if(this.value === "xl" || this.value === "lg"){
                this.series = "8"
                this.seriesP = "4"
            } else{
                this.series = "12"
                this.seriesP = "12"
            }
        }
    }
})
</script>