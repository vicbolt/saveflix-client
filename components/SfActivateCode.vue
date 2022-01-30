<template>
    <div class="sf-activate-code">

        <v-alert v-if="this.error" border="top" type="error" color="red lighten-2" dark class="text-center" dismissible> {{this.error}} </v-alert>
        <v-alert v-if="this.msg" border="top" color="green lighten-2" class="text-center" dark dismissible> {{this.msg}} </v-alert>

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />


        <v-form>
            <h1> ACTIVAR LA CUENTA </h1>
            <v-row>
            <v-col :cols="formulario">
            <v-text-field v-model="email" placeholder="Correo electrónico" />
            <v-row>
                <v-col cols="2">
                    <v-text-field class="code" v-on:keyup="validateA()" v-model="A" />
                </v-col>
                <v-col cols="2">
                    <v-text-field ref="B" class="code" v-on:keyup="validateB()" v-model="B" />
                </v-col>
                <v-col cols="2">
                    <v-text-field ref="C" class="code" v-on:keyup="validateC()" v-model="C" />
                </v-col>
                <v-col cols="2">
                    <v-text-field ref="D" class="code" v-on:keyup="validateD()" v-model="D" />
                </v-col>
                <v-col cols="2">
                    <v-text-field ref="E" class="code" v-on:keyup="validateE()" v-model="E" />
                </v-col>
                <v-col cols="2">
                    <v-text-field ref="F" class="code" v-on:keyup="validateF()" v-model="F" />
                </v-col>
            </v-row>
            
            <v-btn id="boton" block @click="activar"> ACTIVAR </v-btn>
            <img v-if="loading" height="120em" src="@/static/load.gif" class="mb-6" />  


            <div class="enlace">
                <a href="/reactivateCode" :style="enlace"> ¿No ha recibido el código? Haga click aquí para reenviarlo </a>
            </div>
            <div class="enlace">
                <a href="/logIn" :style="enlace"> Iniciar sesión </a>
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
            A: "",
            B: "",
            C: "",
            D: "",
            E: "",
            F: "",
            code: "",
            error: "",
            msg: "",
            formulario: "",
            enlace: "",
            loading: false,
            value: this.$vuetify.breakpoint.name,

        }
    },

    created(){
        this.responsive()
    },

    beforeMount(){
        
        const email = localStorage.getItem('email')
        if(email){
            this.email = email  
        } else {
            this.email = ""
        }
        
    },

    watch: {
        '$vuetify.breakpoint.name'(value){
            console.log(value)

            if( value === "xl" || value === "lg"){
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

        async activar(){
            try{
                
                this.loading = true

                const code = this.A + this.B + this.C + this.D + this.E + this.F

                this.code = code

                const config = require('../config')

                const body = JSON.stringify({
                    email: this.email,
                    code: this.code
                })

                const res = await fetch(config.hostname + 'api/user/activateCode', {
                    method: 'post',
                    headers:{
                        'Content-Type': 'application/json',
                    }, 
                    body: body,
                })

                const data = await res.json()

                if(data.error){
                    this.loading = false
                    return this.error = data.error
                }

                
                this.msg = this.msg = "La cuenta ha sido activada, inicie sesión para continuar"

                setTimeout(() => {
                    this.$router.push('/logIn');
                }, 2000);


                }catch (error) {
                    return console.log(error)
                } 
            },

            validateA(){

                if(this.A.length >= 1){
                    const slice = this.A.slice(0, 1)
                    this.A = slice

                    const nextField = this.$refs.B
                    nextField.focus()
                }
            },


            validateB(){

                if(this.B.length >= 1){
                    const slice = this.B.slice(0, 1)
                    this.B = slice

                    const nextField = this.$refs.C
                    nextField.focus()
                }
            },


            validateC(){

                if(this.C.length >= 1){
                    const slice = this.C.slice(0, 1)
                    this.C = slice

                    const nextField = this.$refs.D
                    nextField.focus()
                }
            },


            validateD(){

                if(this.D.length >= 1){
                    const slice = this.D.slice(0, 1)
                    this.D = slice

                    const nextField = this.$refs.E
                    nextField.focus()
                }
            },


            validateE(){

                if(this.E.length >= 1){
                    const slice = this.E.slice(0, 1)
                    this.E = slice

                    const nextField = this.$refs.F
                    nextField.focus()
                }
            },


            validateF(){

                if(this.F.length >= 1){
                    const slice = this.F.slice(0, 1)
                    this.F = slice
                }
            },
        },
    })

</script>




<style scoped>

img{
    margin-top: 10px;
    display: block;
    margin: auto;
}

.code{
    display: flex;
    font-size: 35px;
    text-align-last: center;
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