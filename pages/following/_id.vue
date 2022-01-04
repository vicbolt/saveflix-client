<template>
    <div class="sf-mi-ranking" v-if="visible">

        <v-row>
            <v-col cols=12>
            <v-card elevation="2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="background-color:rgb(229,9,20)">
                    <v-icon class="mr-4" size="30"> mdi-account-heart</v-icon>
                    <v-card-title class="font-weight-black" style="font-size: 25px"> SIGUIENDO </v-card-title>
                </v-app-bar>

                <v-card-text>
                        <div v-for="seguidor in followers" :key="seguidor.id" @click="goTo(seguidor._id)" id="background" style="cursor:pointer"  class="container-movies mb-5">
                            <v-row style="background-color: rgb(0,0,0,0.9); cursor: pointer" class="mr-0">

                                <v-col cols="3">
                                    <v-img class="imagen" style="border-radius:100%" :src="seguidor.avatar" width="70px" height="70px" />
                                </v-col>
                                <v-col cols="9">
                                    <p class="mt-5 ml-n4 font-weight-black" style="font-size: 18px; color:white;"> {{seguidor.username}} </p>
                                </v-col> 
                                <div>
                                    <v-divider></v-divider>
                                </div>
                            </v-row>
                        </div>
                </v-card-text>         
            </v-card>
            </v-col>
        </v-row>

    </div>
</template>


<script>
export default ({

    data() {
        return {
            visible: true,
            followers: [],
            userId: "",
        }
    },

    async beforeMount() {
        const token = localStorage.getItem("token")
            if(!token){
                this.visible = false
                return this.$router.push("logIn")
            }

        await this.loadFollowers()
    },

    methods: {

        async loadFollowers(){
            try{

                const config = require('/config')

                const userId = localStorage.getItem("ownerId")
                this.userId = userId

                const res = await fetch(config.hostname + `api/user/following/${userId}`)

                const data = await res.json()
                if(data.error){
                    console.log(data.error)
                }

                const seguidores = data.seguidores

                for(const seguidor of seguidores){
                    this.followers.push(seguidor)
                }

            }catch(error){
                return console.log(error)
            }
        },

        goTo(seguidorId){

            const user = JSON.parse(localStorage.getItem("userId"))

            localStorage.setItem("ownerId", seguidorId)

            console.log("yo:",user, "raquel:",seguidorId)

            if( user === seguidorId){
                this.$router.push(`/miPerfil/${seguidorId}`)
            } else {
                this.$router.push(`/perfil/${seguidorId}`)
            }
            
        }
    }
    
})
</script>



<style scoped>

#fondo{
    background-color: white;
    color: black;
    padding: 3px;
    border-radius: 2%;
}

#background{
    background-size: 100% ;
}


#background:hover{
    zoom: 130%;
}

</style>