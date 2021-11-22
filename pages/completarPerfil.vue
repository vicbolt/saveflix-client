<template>
    <div class="sf-login-form">

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form >
            <h1> COMPLETAR PERFIL </h1>
            <v-row>
            <v-col 2></v-col>
            <v-col cols="5">
            <v-text-field placeholder="Nombre de usuario" outlined class="mb-n4"/>
            <v-select label="País de residencia" outlined>
                <option> {{countries}} </option>
            </v-select>
            <div >
            <v-file-input outlined placeholder="AVATAR" prepend-icon="" id="image" class="avatar"></v-file-input> 
            </div>
            <v-btn id="boton" block color="rgb(229,9,20)"> GUARDAR</v-btn>

            <div class="enlace">
            </div>
            </v-col>
           

            <v-col 2></v-col>
            </v-row>

        </v-form>

    </div>
</template>


<script>

export default ({
    layout: 'auth',
    data(){
        return{
            countries: ['España', 'Portugal']
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

h1{
    margin-bottom: 5px;
    text-align: center;
}

.avatar{
    width: 120px;
    height: 120px;
    border-radius: 100%;
}



</style>