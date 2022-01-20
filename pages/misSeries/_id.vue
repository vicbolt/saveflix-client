<template>
    <div class="sf-mis-series" v-if="visible">

        <v-card elevation="2">
                <v-app-bar shaped>
                    <v-icon class="mr-4" size="35"> mdi-television-classic </v-icon>
                    <v-card-title class="font-weight-black mt-1" style="font-size:25px"> MIS SERIES </v-card-title>
                    <v-spacer></v-spacer>
                    <v-btn color="rgb(229,9,20)" @click="addSerial">AÑADIR SERIE</v-btn>
                </v-app-bar>

            <v-card-text>
                <h1 v-if="noSeries === true"> NO TIENES SERIES AÚN</h1>
                <div v-if="loading" style="text-align:center">
                    <img height="120em" src="@/static/load.gif" class="mb-6" />  
                </div>
                <v-row v-if="noSeries === false">
                    <v-col :cols="series" @click="goTo(serial._id)" v-for="serial in serials" :key="serial.id">
                        <v-list-item>
                            <v-list-item-content style="cursor: pointer" class="fondo">
                                <v-img :height="size" :src="serial.image" />
                                <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:20px"> {{serial.title}} </v-list-item-title>
                                    <v-progress-linear color="rgb(229,9,20)" v-model="serial.score" height="25">
                                        <strong> {{ Math.ceil(serial.score) }} <v-icon color="white" size="15"> mdi-heart </v-icon> </strong>
                                        </v-progress-linear>
                                <v-btn class="boton">ver mas...</v-btn>
                            </v-list-item-content>
                        </v-list-item>
                    </v-col>
                </v-row>
            </v-card-text>
                    

        </v-card>


    </div>
</template>

<script>

export default ({
    layout: "serialP",
    
    data() {
      return {
          visible: true,
          serials: [],
          noSeries: "",
          size: "360px",
          series: "4",
          loading: false
      }  
    },

    async beforeMount(){
        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")
        }
        this.loading = true
        await this.loadSerials()
        this.loading = false
    },

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)

            if( value === "xl"){
                this.series = "4"
                this.size = "360px"
                
            } else if(value === "lg" || value === "md" ){
                this.series = "6"
                this.size = "820px"
                
            } else if(value === "sm" || value === "xs") {
                this.series = "12"
                this.size= "960px"

            }
        }
    },

    methods: {
        addSerial(){
            this.$router.push('/addSerial')
        },

        async loadSerials(){
            try{
                const strParamsId = localStorage.getItem('userId')

                const paramsId = JSON.parse(strParamsId)

                const config = require('/config')

                const res = await fetch(config.hostname + `api/serial/getAll/${paramsId}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                this.serials = []
                for(const serial of data.serials){
                    this.serials.push(serial)
                }

                if(this.serials.length === 0){
                    this.noSeries = true
                } else {
                    this.noSeries = false
                }

            }catch(error){
                return console.log(error)
            }
        },

        goTo(serialId){
            localStorage.setItem("postId", serialId)
            this.$router.push(`/details/${serialId}`)
        }
    }
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