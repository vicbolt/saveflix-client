<template>
    <div class="sf-serialP">

        <v-card elevation="2" shaped class="mb-5">
            <v-app-bar>
                <v-row>
                    <v-col cols="9">
                        <v-app-bar-title class="font-weight-black mt-2" style="font-size: 15px"> <v-icon > mdi-clock</v-icon> SERIES PENDIENTES </v-app-bar-title>
                    </v-col>
                    <v-col cols="3">
                        <v-btn class="ml-n4" color="rgb(229,9,20)" style="color:white" @click="goTo()"> AÑADIR</v-btn>
                    </v-col>
                </v-row>
            </v-app-bar>

                <v-card-text>
                    <v-row>
                        <v-col cols="3" v-for="serial in serials" :key="serial._id" class="mb-7">
                                <v-list-item-content style="cursor: pointer" class="mb-n12" @click="go(serial._id)">
                                    <v-img height="130px" style="border: 2px solid white" :src="serial.image" />
                                    <v-list-item-title class="text-center mt-2 font-weight-black" style="font-size:11px"> {{serial.title}} </v-list-item-title>
                                </v-list-item-content>
                        </v-col>
                    </v-row>
                </v-card-text>

        </v-card>
    </div>
</template>

<script>

export default({
    data(){
        return{
            serials: []
        }
    },

    async beforeMount(){
        await this.loadSerialPendientes()
    },

    methods: {

        async loadSerialPendientes(){
            try{

                const id = JSON.parse(localStorage.getItem("userId"))

                const config = require('../config')

                const res = await fetch(config.hostname + `api/serialPendiente/getAll/${id}`)

                const data = await res.json()
                if(data.error){
                    return console.log(data.error)
                }

                const serialsP = data.serialsP

                for(const serial of serialsP){
                    this.serials.push(serial)
                }

            }catch(error){
                return console.log(error)
            }
        },

        goTo(){
            this.$router.push('/addSerialP')
        },

        go(id){
            this.$router.push(`/pendienteDetails/${id}`)
        }
    }

})
</script>
