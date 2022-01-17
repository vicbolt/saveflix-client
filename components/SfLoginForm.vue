<template>
    <div class="sf-login-form mt-4">

            <v-alert v-if="this.error" class="text-center" dismissible type="error" border="top" color="red"  dark> {{this.error}} </v-alert>
            <v-alert v-if="this.msg" class="text-center" dismissible type="error" border="top" color="green"  dark> {{this.msg}} </v-alert>

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form>
            <h1 class="mb-4"> INICIO DE SESIÓN </h1>
            <v-row>
                <v-col :cols="formulario">
                <v-text-field v-model="email" placeholder="Correo electrónico" outlined class="mb-n4"/>
                <v-text-field type="password" v-model="password" placeholder="Contraseña" outlined class="mb-n3"/>  
                <v-btn id="boton" :disabled="active" block @click="login"> INICIAR SESIÓN</v-btn>

                <div class="enlace">
                <a href="/signUp" :style="enlace"> ¿No tienes una cuenta? Regístrate para continuar</a>
                </div>

                <div class="enlace mt-2">
                    <a href="/activateCode" :style="enlace" > Haga click aquí para activar una cuenta</a>
                </div>
                </v-col>
            </v-row>
        </v-form>

    </div>
</template>

<script>
export default ({
    data(){
        return{
            email: "",
            password: "",
            error: "",
            msg: "",
            active: false,
            formulario: "",
            enlace: "",
        }
    },

    beforeMount(){

        const email = localStorage.getItem("email")
        if(email){
            this.email = email
        } else {
            this.email = ""
        }
    },

        watch: {
            '$vuetify.breakpoint.name'(value){
                console.log(value)

                if( value === "xl" || value === "lg" || value === 'md'){
                    this.formulario = "6 offset-3"
                    this.enlace = "font-size: 15px"
                } else {
                    this.formulario = "10 offset-1"
                    this.enlace = "font-size: 12px"
                }
            
        }
    },

    methods: {

        async login(){
            try{
                this.active = true

                const config = require('../config')

                const body = JSON.stringify({
                    email: this.email,
                    password: this.password
                })

                const res = await fetch(config.hostname + 'api/user/logIn', {
                    method: 'post',
                    headers:{
                        'Content-Type': 'application/json',
                    }, 
                    body: body,
                })

                const data = await res.json()
                if(data.error){
                    
                    this.error = data.error

                    setTimeout(() => {
                        this.$router.go()
                    }, 2000);
                }

                if(data.token){
                    localStorage.setItem('token', JSON.stringify(data.token))
                    localStorage.setItem('userId', JSON.stringify(data.userId))
                    
                }

                localStorage.removeItem("email")

                setTimeout(() => {
                    this.$router.push(`/explorar/${userId}`);
                }, 2000);

                }catch (error) {
                    return console.log(error)
                } 
            }
        },

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