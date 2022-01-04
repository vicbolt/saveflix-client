<template>
    <div class="sf-Activate-Code">

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form>
            <h1> ACTIVAR LA CUENTA </h1>
            <v-row>
            <v-col></v-col>
            <v-col cols="5">
            <v-text-field v-model="email" placeholder="Correo electrónico" outlined class="mb-n4"/>
            <v-text-field v-model="code" placeholder="Código enviado a su correo electrónico" outlined class="mb-n3"/>  
            <v-btn id="boton" block @click="activar"> ACTIVAR</v-btn>

            <div class="enlace" @click="reactivarCode()">
            <a> ¿No ha recibido el código? Haga click aquí para reenviarlo </a>
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
            code: "",
            visible: true,
        }
    },

    beforeMount(){
        
        const email = localStorage.getItem('email')
        this.email = email
    },

    methods: {

        async activar(){
            try{
                const config = require('../config')

                const body = JSON.stringify({
                    email: this.email,
                    code: this.code
                })

                console.log(body)

                const res = await fetch(config.hostname + 'api/user/activateCode', {
                    method: 'post',
                    headers:{
                        'Content-Type': 'application/json',
                    }, 
                    body: body,
                })

                const data = await res.json()

                if(data.error){
                    return alert(data.error)
                }

                alert("¡La cuenta se ha activado!")
                this.$router.push('/logIn')


                }catch (error) {
                    return console.log(error)
                } 
            },

            async reactivarCode(){
                try{

                    const config = require('../config')

                    const body = JSON.stringify({
                        email: this.email
                    })
                

                    const res = await fetch(config.hostname + 'api/user/reactivateCode', {
                        method: 'post',
                        headers: {
                            'Content-Type': 'appplication/json'
                        },
                        body: body
                    })

                    const data = await res.json()
                    if(data.error){
                        alert(data.error)
                    } else {
                        return alert("El código ha sido enviado a su correo electrónico")
                    }

                }catch(error){
                    return console.log(error)
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