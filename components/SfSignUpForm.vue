<template>
    <div class="sf-signUp-form">

        <v-alert v-if="this.error" type="error" border="top" color="red lighten-2" dark> {{this.error}} </v-alert>
        <v-alert v-if="this.msg" border="top" color="red lighten-2" dark> {{this.msg}} </v-alert>

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form >
            <h1 class="mb-2"> REGISTRO </h1>
            <v-row>
            <v-col cols="6 offset-3">
            <v-text-field v-model="email" type="email" placeholder="Correo Electrónico" outlined class="mb-n4"/>
            <v-text-field v-model="username" placeholder="Nombre de usuario" outlined class="mb-n2"/>
            <v-text-field v-model="password" type="password" placeholder="Contraseña" outlined class="mb-n4"/>
            <v-text-field v-model="password2" type="password" placeholder="Repita la contraseña" outlined class="mb-n2"/>
            <v-row>
                <v-col cols="2">
                    <v-file-input v-model="avatar"  height="80px" outlined prepend-icon="" placeholder="AVATAR" filled> </v-file-input>
                </v-col>
                <v-col cols="9"></v-col>
            </v-row>
            
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
            avatar: undefined,
            error: "",
            msg: ""
        }
    },

    methods: {
        async submit(){

                const formData = new FormData()
                    formData.enctype = "multipart/form-data",
                    formData.append('email', this.email)
                    formData.append('username', this.username)
                    formData.append('password', this.password)
                    formData.append('password2', this.password2)
                    formData.append('avatar', this.avatar)

                try{

                const config = require('../config')

                const res = await fetch(config.hostname + 'api/user/signUp', {
                    method: 'post',
                    body: formData
                })

                const data = await res.json()
                    if(data.error){
                        return this.error = data.error
                    } else {
                        localStorage.setItem('email', this.email)
                        this.msg = "La cuenta ha sido activada, inicie sesión para continuar"
                        setTimeout(() => {
                            this.$router.push('/activateCode')
                        }, 2000);
                        
                    }

                
                


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