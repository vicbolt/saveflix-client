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
                <v-row v-if="noSeries === false">
                    <v-col @click="goTo(serial._id)" cols=4 v-for="serial in serials" :key="serial.id">
                        <v-list-item>
                            <v-list-item-content style="cursor: pointer" class="fondo">
                                <v-img height="280px" :src="serial.image" />
                                <v-list-item-title class="text-center mt-2 font-weight-black"> {{serial.title}} </v-list-item-title>
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
      }  
    },

    async beforeMount(){
        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")
        }
        await this.loadSerials()
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
                    return alert(data.error)
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