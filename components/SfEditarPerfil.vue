<template>
<div class="sf-edit-perfil">

     <v-alert v-if="this.error" type="error" border="top" class="text-center" dismissible color="red" dark> {{this.error}} </v-alert>
        <v-card>
            <v-card-title class="text-h5 white" style="color:black" > EDITAR PERFIL </v-card-title>
            <v-card-text>
                <h6 class="mt-4">NOMBRE DE USUARIO</h6>
                <v-text-field v-text-field v-model="username" :placeholder="username" class="mb-n4" outlined/>
                <h6>CORREO ELECTRÓNICO</h6>
                <v-text-field v-model="email" :placeholder="email" class="mb-n4" outlined/>
                <h6>CONTRASEÑA</h6>
                <v-row>
                    <v-col cols="11">
                        <v-text-field v-model="password" :placeholder="password" :type="type" class="mb-n8" outlined/>
                    </v-col>
                    <v-col cols="1">
                        <v-icon color="white" size="30" class="ml-n4 mt-3" @click="show"> mdi-eye </v-icon>
                    </v-col>
                </v-row>                      
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn id="boton" type="submit" color="rgb(229,9,20)" block @click="guardar"> GUARDAR CAMBIOS </v-btn>
            </v-card-actions>
    </v-card>
</div>
</template>

<script>

export default ({
    data() {
        return{
            username: "",
            email: "",
            password: "",
            type: "password",
            error: "",
        }
    },

    async beforeMount(){

        try{
                const config = require('../config')

                const strId = localStorage.getItem('userId')
                const id = JSON.parse(strId)

                const res = await fetch(config.hostname + `api/user/getOne/${id}`)

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                this.username = data.user.username
                this.email = data.user.email
                this.password = ""
                

            }catch(error){
                return console.log(error)
            }
        
    },

    methods: {

        show(){
            if(this.type === "password"){
                this.type = "text"
            } else {
                this.type = "password"
            }
        },

        async guardar(){
            try{
                const config = require('../config')
                
                const body = JSON.stringify({
                    username: this.username,
                    email: this.email,
                    password: this.password
                })

                const strId = localStorage.getItem('userId')
                const id = JSON.parse(strId)

                const res = await fetch(config.hostname + `api/user/update/${id}`,{
                    method: 'put',
                    headers:{
                        'Content-Type': 'application/json'
                    },
                    body,
                })

                const data = await res.json()
                if(data.error){
                    return this.error = data.error
                }

                alert('Los cambios se han guardado correctamente')

                this.$router.push(`/explorar/${id}`)

            }catch(error){
                return console.log(error)
            }
        }
    }

})
</script>
