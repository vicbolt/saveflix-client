<template>
    <div class="sf-login-form">

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form >
            <h1 class="mb-2"> REGISTRO </h1>
            <v-row>
            <v-col cols="6 offset-3">
            <v-text-field v-model="email" type="email" placeholder="Correo Electrónico" outlined class="mb-n4"/>
            <v-text-field v-model="username" placeholder="Nombre de usuario" outlined class="mb-n2"/>
            <v-text-field v-model="password" type="password" placeholder="Contraseña" outlined class="mb-n4"/>
            <v-text-field v-model="password2" type="password" placeholder="Repita la contraseña" outlined class="mb-n2"/>
               
            <v-btn id="boton" @click="submit" block> REGISTRARME </v-btn>

            <div class="enlace">
            <a href="/logIn"> ¿Ya tienes una cuenta? ¡Inicia sesión para continuar!</a>
            </div>
            </v-col>
        
            </v-row>

        </v-form>

    </div>
</template>

<script>

export default ({
    data() {
        return{
            email: "",
            username: "",
            password: "",
            password2: "",
        }
    },

    methods: {
        async submit(){
            try{
                const body = JSON.stringify({
                    email: this.email,
                    username: this.username,
                    password: this.password,
                    password2: this.password2
                })

                const res = await fetch('http://localhost:4500/api/user/signUp', {
                    method: 'post',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                    return alert(data.error)
                }

                alert('El usuario se ha registrado con éxito')
                this.$router.push('/logIn')

            } catch(error){
                return res.status(500).json(error)
            }
        },

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