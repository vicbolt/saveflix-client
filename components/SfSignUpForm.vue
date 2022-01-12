<template>
    <div class="sf-signUp-form">

        <v-alert v-if="this.error" type="error" border="top" color="red lighten-2" dark> {{this.error}} </v-alert>
        <v-alert v-if="this.msg" border="top" type="success" color="red lighten-2" dark> {{this.msg}} </v-alert>

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
                <v-col cols="8" id="center">
                    <div class="file-container">
                        <h5 v-if="!url" id="h5">SUBIR AVATAR</h5>
                        <img id="img" v-if="url" :src="url" alt="Image not found" @click="picAgain">
                        <input v-if="!url" type="file" @change="showImg" ref="file" id="imgBtn" class="mt-3" style="background: red; border: 2px solid white; overflow: hidden"/>
                    </div>                   
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
            msg: "",
            url: "",
        }
    },

    methods: {
        async submit(){

                try{

                const body = JSON.stringify({
                    title:  this.title,
                    director: this.director,
                    description: this.description,
                    score: this.score,
                    userId: userId,
                    image: this.url
                })

                const config = require('../config')

                const res = await fetch(config.hostname + 'api/user/signUp', {
                    method: 'post',
                    headers: {
                        "Content-Type" : "application/json"
                    },
                    body
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