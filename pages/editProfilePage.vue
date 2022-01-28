<template>
    <div class="sf-edit-profile-page" v-if="visible">

        <v-alert v-if="this.error" class="text-center" type="error" border="top" color="red" dismissible > {{this.error}} </v-alert>
        <v-alert v-if="this.msg" class="text-center" type="error" border="top" color="green" dismissible > {{this.msg}} </v-alert>

        <v-row>
            <v-col cols="8 offset-2">
                <v-app-bar shaped class="d-flex justify-center mb-2" style="background-color:rgb(229,9,20)">
                    <v-icon class="mr-4" size="30"> mdi-account-edit </v-icon>
                    <v-card-title class="font-weight-black" style="font-size: 25px"> EDITAR PERFIL </v-card-title>
                </v-app-bar>
            </v-col>
        </v-row>

        <v-form>
            <v-row>
            <v-col cols="8 offset-2">
            <h3 class="mb-1">CORREO ELECTRÓNICO</h3>
            <v-alert v-if="this.error1" class="text-center" type="error" border="top" color="red" dismissible > {{this.error1}} </v-alert>
            <v-alert v-if="this.msg1" type="error" class="text-center" border="top" color="green" dismissible > {{this.msg1}} </v-alert>
            <v-text-field v-model="email" type="email" placeholder="Correo Electrónico" class=""></v-text-field>
            <v-btn @click="saveEmail()" block color="red" class="mb-8"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>

            <h3 class="mb-1">NOMBRE DE USUARIO</h3>
            <v-alert v-if="this.error2" class="text-center" type="error" border="top" color="red" dismissible > {{this.error2}} </v-alert>
            <v-alert v-if="this.msg2" class="text-center" type="info" border="top" color="green" dismissible > {{this.msg2}} </v-alert>
            <v-text-field v-model="username" placeholder="Nombre de usuario" class="mb-4"/>
            <v-btn @click="saveUsername()"  block color="red" class="mb-8"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>
            
            
            <h3 class="mb-1">CONTRASEÑA</h3>
            <v-alert v-if="this.error3" class="text-center" type="error" border="top" color="red" dismissible > {{this.error3}} </v-alert>
            <v-alert v-if="this.msg3" class="text-center" type="error" border="top" color="green" dismissible > {{this.msg3}} </v-alert>
            <v-text-field v-model="currentPassword" type="password" placeholder="Contraseña actual" class="mb-n2"/>
            <v-text-field v-model="newPassword" type="password" placeholder="Nueva contraseña" class="mb-n2"/>
            <v-text-field v-model="newPassword2" type="password" placeholder="Repita la nueva contraseña " class="mb-2"/>
            <v-btn @click="savePassword()" block color="red" class="mb-8"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>

            <h3 class="mb-4">AVATAR</h3>
            <v-alert v-if="this.error4" class="text-center" type="error" border="top" color="red" dismissible > {{this.error4}} </v-alert>
            <v-alert v-if="this.msg4" class="text-center" type="error" border="top" color="green" dismissible > {{this.msg4}} </v-alert>
            <div class="file-container mb-4">
                <h5 v-if="!url" id="h5">SUBIR AVATAR</h5>
                <img id="img" v-if="url" :src="url" alt="Image not found" @click="picAgain">
                <input v-if="!url" type="file" @change="showImg" ref="file" id="imgBtn" class="mt-3" style="background: red; border: 2px solid white; overflow: hidden"/>
            </div>   
            <v-btn id="boton" @click="saveAvatar()"  block color="red"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>

            <h3 class="mb-1">BORRAR PERFIL</h3>
            <p class="mb-3" style="font-size: 12px"> Al pulsar sobre "Borrar Perfil" se eliminará la cuenta y los datos asociados a ella de forma irreversible</p>
            <v-alert v-if="alertMsg" prominent type="error">
                <v-row align="center">
                    <v-col class="grow">
                           <strong>{{this.alertMsg}}</strong>
                    </v-col>
                    <v-col class="shrink">
                        <v-btn @click="borrarPerfil">ACEPTAR</v-btn>
                    </v-col>
                </v-row>
            </v-alert>

            <v-btn id="boton" @click="securityMsg"  block color="red"> BORRAR PERFIL </v-btn>
            </v-col>
        
            </v-row>

        </v-form>


    </div>

</template>

<script>

export default ({
    layout: "soloMenu",

    data(){
        return{
            visible: true,
            email: "",
            username: "",
            currentPassword: "",
            newPassword: "",
            newPassword2: "",
            url: "",
            error: "",
            error1: "",
            error2: "",
            error3: "",
            error4: "",
            error5: "",
            msg: "",
            msg1:"",
            msg2: "",
            msg3: "",
            msg4: "",
            msg5: "",
            alertMsg: "",
            active: false
        }
    },

    async beforeMount(){
        const token = localStorage.getItem("token")
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")
        }

        await this.loadUserDetails()
    },

    methods:{

        async loadUserDetails(){
            try{
            const config = require('/config')
            
            const userId = JSON.parse(localStorage.getItem("userId"))

            const res = await fetch(config.hostname + `api/user/getOne/${userId}`)

            const data = await res.json()
            if(data.error){
                console.log(data.error)
            }

            this.email = data.user.email
            this.username = data.user.username
            this.url = data.user.avatar
            

            } catch(error){
                return console.log(error)
            }
        },

        async saveEmail(){
            try{
                const config = require('/config')

                const id = JSON.parse(localStorage.getItem("userId"))

                const body = JSON.stringify({
                    email: this.email
                })
                
                const res = await fetch(config.hostname + `api/user/saveEmail/${id}`,{
                    method: "put",
                    headers: {
                        'Content-Type': "application/json"
                    },
                    body,
                })

                const data = await res.json()

                    localStorage.setItem("email", this.email)
                    localStorage.setItem("oldEmail", data.user.email)
                    this.msg = "Inserte el código enviado a su email"
                    setTimeout(() => {
                        this.$router.push(`/activateEmail`)
                    }, 2000);


            }catch(error){
                return console.log(error)
            }
        },

        async saveUsername(){
            try{
                const config = require('/config')

                const id = JSON.parse(localStorage.getItem("userId"))

                const body = JSON.stringify({
                    username: this.username,
                })
                
                const res = await fetch(config.hostname + `api/user/saveUsername/${id}`,{
                    method: "put",
                    headers: {
                        'Content-Type': "application/json"
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }
                this.msg2 = "El nombre de usuario se ha cambiado correctamente"


            }catch(error){
                return console.log(error)
            }
        },

        async savePassword(){
            try{
                const config = require('/config')

                const id = JSON.parse(localStorage.getItem("userId"))

                const body = JSON.stringify({
                    password: this.currentPassword,
                    newPassword: this.newPassword,
                    newPassword2: this.newPassword2
                })
                
                const res = await fetch(config.hostname + `api/user/savePassword/${id}`,{
                    method: "put",
                    headers: {
                        'Content-Type': "application/json"
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                   return this.error3 = data.error
                }

                this.msg3 = "La contraseña se ha cambiado correctamente"

                setTimeout(() => {
                    this.$router.push(`/miPerfil/${id}`)
                }, 2000);

            }catch(error){
                return console.log(error)
            }
        },

        async saveAvatar(){
            try{
                const config = require('/config')

                const id = JSON.parse(localStorage.getItem("userId"))

                const body = JSON.stringify({
                    avatar: this.url
                })

                const res = await fetch(config.hostname + `api/user/saveAvatar/${id}`,{
                    method: "put",
                    headers: {
                        'Content-Type': "application/json"
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                    return this.error4 = data.error
                }

                this.msg4 = "El avatar del usuario se ha cambiado con éxito"

                setTimeout(() => {
                    this.$router.push(`/miPerfil/${id}`)
                }, 2000);

            }catch(error){
                return console.log(error)
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

        securityMsg(){
            try{

                this.alertMsg = "¿Estás segúro de que quieres borrar tu perfil de Saveflix?"

            }catch(error){
                return console.log(error)
            }
        },

        async borrarPerfil(){
            try{

                const config = require('/config')

                const id = JSON.parse(localStorage.getItem("userId"))

                const res = await fetch(config.hostname + `api/user/remove/${id}`,{
                    method: "delete",
                    headers: {
                        'Content-Type': "application/json"
                    },
                })

                const data = await res.json()
                if(data.error){
                    return this.error5 = data.error
                }

                this.msg5 = "El perfil se ha borrado correctamente, esperamos volver a verte pronto"

                localStorage.clear()

                setTimeout(() => {
                    this.$router.push('/logIn')
                }, 2000);

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

</style>
