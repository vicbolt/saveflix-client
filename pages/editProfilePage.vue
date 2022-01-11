<template>
    <div class="sf-edit-profile-page" v-if="visible">

        <v-alert v-if="this.error" type="error" border="top" color="red" > {{this.error}} </v-alert>

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
            <v-text-field v-model="email" type="email" placeholder="Correo Electrónico" class=""></v-text-field>
            <v-btn @click="saveEmail()" block color="red" class="mb-8"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>

            <h3 class="mb-1">NOMBRE DE USUARIO</h3>
            <v-text-field v-model="username" placeholder="Nombre de usuario" class="mb-4"/>
            <v-btn @click="saveUsername()"  block color="red" class="mb-8"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>
            
            
            <h3 class="mb-1">CONTRASEÑA</h3>
            <v-text-field v-model="currentPassword" type="password" placeholder="Contraseña actual" class="mb-n2"/>
            <v-text-field v-model="newPassword" type="password" placeholder="Nueva contraseña" class="mb-n2"/>
            <v-text-field v-model="newPassword2" type="password" placeholder="Repita la nueva contraseña " class="mb-2"/>
            <v-btn @click="savePassword()" block color="red" class="mb-8"> GUARDAR </v-btn>

            <v-divider class="mt-7 mb-7"></v-divider>

            <h3 class="mb-4">AVATAR</h3>
            <v-file-input prepend-icon="mdi-face-man-profile" :src="avatar" placeholder="AVATAR" filled/> 
            
            <v-btn id="boton" @click="saveAvatar()"  block color="red"> GUARDAR </v-btn>
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
            avatar: "",
            error: "",
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
            this.avatar = data.user.avatar
            

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
                    alert("Inserte el código enviado a su email")
                    this.$router.push('/activateEmail')


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

                alert('El usuario se ha modificado')
                return this.$router.push(`/miPerfil/${id}`)

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
                   return this.error = data.error
                }

                alert('El usuario se ha modificado')
                return this.$router.push(`/miPerfil/${id}`)

            }catch(error){
                return console.log(error)
            }
        },

        async saveAvatar(){
            try{
                const config = require('/config')

                const id = JSON.parse(localStorage.getItem("userId"))

                const formData = new FormData()
                    formData.enctype = "multipart/form-data",
                    formData.append('avatar', this.avatar)

                
                const res = await fetch(config.hostname + `api/user/saveAvatar/${id}`,{
                    method: "put",
                    headers: {
                        'Content-Type': "application/json"
                    },
                    body: formData
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                alert('El usuario se ha modificado')
                return this.$router.push(`/miPerfil/${id}`)

            }catch(error){
                return console.log(error)
            }
        },


    }
    
})
</script>
