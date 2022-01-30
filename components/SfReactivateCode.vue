<template>
    <div class="sf-reactivate-code">

        <v-alert v-if="this.error" class="text-center" dismissible type="error" border="top" color="red" dark> {{this.error}} </v-alert>

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form>
            <h1 :style="fontSize" class="mb-2"> REENVIAR CODIGO DE ACTIVACIÓN </h1>
            <v-row>
                <v-col :cols="formulario">
                    <v-text-field v-model="email" placeholder="Correo electrónico" outlined class="mb-n4"/>
                    <v-btn id="boton" block @click="reactivateCode"> ENVIAR DE NUEVO EL CODIGO </v-btn>
                    <img v-if="loading" height="120em" src="@/static/load.gif" class="mb-6" />  
                </v-col>
            </v-row>

            <div class="enlace">
                <a href="/logIn"> INICIO DE SESIÓN</a>
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
            loading: false,
            formulario: "",
            fontSize: "",
            enlace: "",
            value: this.$vuetify.breakpoint.name,

        }
    },

    created(){
        this.responsive()
    },


    beforeMount(){
        
        const email = localStorage.getItem('email')
        this.email = email
    },

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)

            if( value === "xl" || value === "lg"){
                this.formulario = "6 offset-3"
                this.fontSize = "font-size: 15px"
            } else {
                this.formulario = "10 offset-1"
                this.fontSize = "font-size: 12px"
            }
        }
    },

    methods: {

        responsive(){
            if( this.value === "xl" || this.value === "lg"){
                this.formulario = "6 offset-3"
                this.fontSize = "font-size: 15px"
            } else {
                this.formulario = "10 offset-1"
                this.fontSize = "font-size: 12px"
            }
        },
    
        async reactivateCode(){
            try{
                this.loading = true

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
                    this.loading = false
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