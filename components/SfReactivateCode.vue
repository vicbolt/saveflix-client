<template>
    <div class="sf-reactivate-code">

        <v-alert v-if="this.error" class="text-center" dismissible type="error" border="top" color="red" dark> {{this.error}} </v-alert>

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form>
            <h1> REENVIAR CODIGO DE ACTIVACIÓN </h1>
            <v-row>
            <v-col></v-col>
                <v-col cols="5">
                <v-text-field v-model="email" placeholder="Correo electrónico" outlined class="mb-n4"/>
                <v-btn id="boton" block @click="reactivateCode"> ENVIAR DE NUEVO EL CODIGO </v-btn>
                </v-col>
                <v-col></v-col>
            </v-row>

            <div class="enlace">
                <a href="/logIn"> Inicio de Sesión</a>
            </div>
        </v-form>

    </div>
</template>

<script>
export default ({
    data(){
        return{
            email: "",
            error: "",
        }
    },

    beforeMount(){
        
        const email = localStorage.getItem('email')
        this.email = email
    },

    methods: {
    
        async reactivateCode(){
            try{
                
                const body = JSON.stringify({
                    email: this.email
                })

                const config = require('../config')

                const res = await fetch(config.hostname + 'api/user/reactivateCode', {
                    method: 'post',
                    headers: {
                        'Content-Type' : "application/json"
                    },
                    body: body,
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                alert(`El código ha sido enviado a ${this.email}`)
                this.$router.push('/activateCode')

            }catch(error){
                return console.log(error)
            }
        }
    }


    })

</script>




<style scoped>

img{
    margin-top: 10px;
    display: block;
    margin: auto;
}

.enlace{
    text-align: center;
}

a{
    color: white
}

h1{
    margin-bottom: 5px;
    text-align: center;
}

#boton{
    background-color: rgb(18, 18, 18);
    margin-bottom: 7px;
    border: 2px solid rgb(229,9,20);
}
</style>