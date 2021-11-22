<template>
    <div class="sf-login-form">

        <img height="120em" src="@/assets/images/logotipoWeb.png" class="mb-6" />
        <v-form >
            <h1 class="mb-2"> REGISTRO </h1>
            <v-row>
            <v-col 2></v-col>
            <v-col cols="5">
            <v-text-field placeholder="Correo Electrónico" outlined class="mb-n4"/>
            <v-text-field placeholder="Contraseña" outlined class="mb-n4"/>
            <v-text-field placeholder="Repita la contraseña" outlined class="mb-n2"/>   
            <v-btn id="boton" type="submit" block> REGISTRARME</v-btn>

            <div class="enlace">
            <a href="/logIn"> ¿Ya tienes una cuenta? ¡Inicia sesión para continuar!</a>
            </div>
            </v-col>
           

            <v-col 2></v-col>
            </v-row>

        </v-form>

    </div>
</template>



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