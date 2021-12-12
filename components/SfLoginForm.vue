<template>
    <div class="sf-login-form" v-if="visible">

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form>
            <h1> INICIO DE SESIÓN </h1>
            <v-row>
            <v-col></v-col>
            <v-col cols="5">
            <v-text-field v-model="email" placeholder="Correo electrónico" outlined class="mb-n4"/>
            <v-text-field type="password" v-model="password" placeholder="Contraseña" outlined class="mb-n3"/>  
            <v-btn id="boton" block @click="login"> INICIAR SESIÓN</v-btn>

            <div class="enlace">
            <a href="/signUp"> ¿No tienes una cuenta? Regístrate para continuar</a>
            </div>
            </v-col>

            <v-col></v-col>
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
            visible: true,
        }
    },

    beforeMount(){
        
        const token = localStorage.getItem('token')

        const strId = localStorage.getItem('userId')
        const id = JSON.parse(strId)

        if(token){
            this.visible = false
            this.$router.push(`/explorar/${id}`)
        }
    },

    methods: {

        async login(){
            try{
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
                    alert(data.error)
                    this.$router.push('/logIn')
                }

                if(data.token){
                    localStorage.setItem('token', JSON.stringify(data.token))
                    localStorage.setItem('userId', JSON.stringify(data.userId))
                    
                }

                this.$router.push('/')

                


                }catch (error) {
                    return res.json(error)
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