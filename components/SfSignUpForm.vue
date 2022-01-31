<template>
    <div class="sf-signUp-form">

        <v-alert v-if="this.error" type="error" class="text-center" dismissible border="top" color="red" dark> {{this.error}} </v-alert>
        <v-alert v-if="this.msg" border="top" type="success" class="text-center" dismissible color="green" dark> {{this.msg}} </v-alert>

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form >
            <h1 class="mb-2"> REGISTRO </h1>

            <v-row>
                <v-col :cols="formulario">
                    <v-text-field v-model="email" type="email" placeholder="Correo Electrónico" outlined class="mb-n4"/>
                    <v-text-field v-model="username" placeholder="Nombre de usuario" outlined class="mb-n2"/>
                    <v-text-field v-model="password" type="password" placeholder="Contraseña" outlined class="mb-n4"/>
                    <v-text-field v-model="password2" type="password" placeholder="Repita la contraseña" outlined class="mb-n2"/>
                </v-col>
            </v-row>

            <v-row>
                <v-col :cols="formulario" id="center">
                    <div class="file-container">
                        <h5 v-if="!url" id="h5">SUBIR AVATAR</h5>
                        <img id="img" v-if="url" :src="url" alt="Image not found" @click="picAgain">
                        <input v-if="!url" type="file" @change="showImg" ref="file" id="imgBtn" class="mt-3" style="background: red; border: 2px solid white; overflow: hidden"/>
                    </div>                   
                </v-col>
            </v-row>
            
            <v-row>
                <v-col :cols="formulario">
                <v-btn id="boton" :disabled="active" @click="submit"  block> REGISTRARME </v-btn>
                <img v-if="loading" height="120em" src="@/static/load.gif" class="mb-6" />  


                <div class="enlace">
                <a href="/logIn" :style="enlace"> ¿Ya tienes una cuenta? ¡Inicia sesión para continuar!</a>
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
            msg: "",
            url: "",
            active: false,
            formulario: "",
            enlace: "",
            loading: false,
            value: this.$vuetify.breakpoint.name,

        }
    },

    created(){
        this.responsive()
    },

    watch: {
        '$vuetify.breakpoint.name'(value){

            if(value === "xl" || value === "lg"){
                this.formulario = "6 offset-3"
                this.enlace = "font-size: 15px"
            } else {
                this.formulario = "10 offset-1"
                this.enlace = "font-size: 12px"
            }
        }
    },

    methods: {

        responsive(){
            if( this.value === "xl" || this.value === "lg"){
                this.formulario = "6 offset-3"
                this.enlace = "font-size: 15px"
            } else {
                this.formulario = "10 offset-1"
                this.enlace = "font-size: 12px"
            }
        },

        async submit(){
            try{
                this.active = true
                this.loading = true

                const body = JSON.stringify({
                    email: this.email,
                    username: this.username,
                    password: this.password,
                    password2: this.password2,
                    avatar: this.url
                })

                const config = require('../config')

                const res = await fetch(config.hostname + 'api/user/signUp', {
                    method: 'post',
                    headers: {
                        "Content-Type" : "application/json",
                        "Access-Control-Allow-Origin": "*",
                    },
                    body,
                })

                const data = await res.json()
                    if(data.error){
                        this.loading = false
                        this.error = data.error
                        setTimeout(() => {
                            this.$router.go()
                        }, 2000);

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

        showImg(){

            const file = this.$refs.file.files[0]
            const reader = new FileReader()
            reader.onloadend = () => {
                console.log(reader.result)
                this.url = reader.result
                console.log("ESTO ES THIS.URL" ,this.url)
            }
            if(file){
                reader.readAsDataURL(file)
            }
        },

        picAgain(){
            this.url = ""
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

.file-container{
    background-color: rgb(255, 255, 255);
    height: 107px;
    width: 107px;
    border: 2px solid white;
    border-radius: 100%;
    display: flex;
    align-items: center;
    cursor: pointer;
    position: relative;
}

#img{
    border-radius: 100%;
    height: 100px;
    width: 100px;
    cursor: pointer;
}

#imgBtn{
    position: absolute;
    opacity: 0;
    width: 100px;
    height: 100px;
    cursor: pointer;
    z-index: 2;
}

#h5{
    position: center absolute;
    display: flex;
    color: rgb(0, 0, 0);
    z-index: 1;
    margin-left: 4px;
    cursor: pointer;
}

</style>