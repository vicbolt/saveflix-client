<template>
    <div class="sf-menu-nav" v-if="visible">

        <v-app-bar class="barra" elevation="16" rounded >
            <a href="/">
            <img height="50em" width="130em" src="@/assets/images/logotipoWeb.png" />
            </a>
            <div class="ml-5px">
            <v-btn id="Btn" class="font-weight-black ml-15" width="10em" height="50px" href="/home">
            <v-badge color="pink" dot>HOME</v-badge>
            </v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" href="/misPeliculas">MIS PELICULAS</v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" href="/misSeries">MIS SERIES</v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" href="/miRanking">MI RANKING</v-btn>
            <v-btn id="Btn" class="font-weight-black ml-1" width="10em" height="50px" href="/chat">
            <v-badge color="pink" dot>CHAT</v-badge>
            </v-btn>
            </div>

            <v-spacer></v-spacer>


  
            <v-menu :close-on-content-click="false" :nudge-width="400" max-width="250px">
                <template v-slot:activator="{ on }">
                    <v-btn class="mr-10" v-on="on" depressed fab small>
                        <v-avatar size="55" class="mr-3" color="rgb(229,9,20)"> 
                             <img class="avatar" src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/5b8f05fa-b132-4fdb-8c4d-b103a177656c/d2hkfk2-9329643d-295a-4de0-b809-7bc460d023a5.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzViOGYwNWZhLWIxMzItNGZkYi04YzRkLWIxMDNhMTc3NjU2Y1wvZDJoa2ZrMi05MzI5NjQzZC0yOTVhLTRkZTAtYjgwOS03YmM0NjBkMDIzYTUuanBnIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.mbgi2x7zLaYHoJ0pM5bjKk4hNWL7OznlpItGyI__Q6w" alt="avatar">
                        </v-avatar>
                    </v-btn>
                </template>

                <v-card>
                    <v-list>
                        <v-list-item>
                            <v-list-item-avatar size="60" class="mr-5" color="rgb(229,9,20)">
                                <img class="avatar" src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/5b8f05fa-b132-4fdb-8c4d-b103a177656c/d2hkfk2-9329643d-295a-4de0-b809-7bc460d023a5.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzViOGYwNWZhLWIxMzItNGZkYi04YzRkLWIxMDNhMTc3NjU2Y1wvZDJoa2ZrMi05MzI5NjQzZC0yOTVhLTRkZTAtYjgwOS03YmM0NjBkMDIzYTUuanBnIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.mbgi2x7zLaYHoJ0pM5bjKk4hNWL7OznlpItGyI__Q6w" alt="Avatar">
                            </v-list-item-avatar>
                            <v-list-item-title>
                                <v-list-item-title class="font-weight-black"> {{username}} </v-list-item-title>
                                <v-list-item-subtitle> {{email}} </v-list-item-subtitle>
                            </v-list-item-title>
                        </v-list-item>
                    </v-list>

                    <v-divider class="mt-n2 mb-2"></v-divider>

                    <v-list>
                        <v-list-item>
                            <v-row>
                                <v-col cols="12">
                                    <v-btn class="ml-1" width="auto" height="50px" light block> AMIGOS</v-btn>
                                </v-col>
                                <v-col cols="12">
                                    <v-dialog width="500">
                                        <template v-slot:activator="{ on, attrs }">
                                            <v-btn class="ml-1 mt-n3" width="auto" height="50px" light block @click="editarPerfil" v-bind="attrs" v-on="on">EDITAR PERFIL</v-btn>
                                        </template>
                                        <SfEditarPerfil/>
                                    </v-dialog>

                                </v-col>
                                <v-col cols="12">
                                    <v-btn class="ml-1" width="auto" height="50px" color="red" block @click="logOut">CERRAR SESION</v-btn>
                                </v-col>    
                            </v-row>
                        </v-list-item>
                    </v-list>
                </v-card>
            </v-menu>
      
        </v-app-bar>
    </div>
</template>

<script>
export default({
    data() {
        return{
            visible: true,
            username: "",
            email: "",
        }
    },

    async beforeMount(){

        await this.loadInfo()

    },

    methods: {

        async loadInfo(){
            try{
                const token = localStorage.getItem('token')

                if(!token){
                    this.visible = true
                }

                const strId = localStorage.getItem('userId')
                const id = JSON.parse(strId)

                const res = await fetch(`http://localhost:4500/api/user/getOne/${id}`)

                const data = await res.json()
                if(data.error){
                    return alert(data.error)
                }

                
                this.email = data.user.email
                this.username = data.user.username





            } catch(error){
                return alert(error)
            }

        },

        logOut(){
            localStorage.removeItem('token')
            localStorage.removeItem('userId')
            
            this.$router.push('/logIn')
            
        },
        editarPerfil(){
            this.$route.router.go('/editarPerfil')
        }
    }
})
</script>


<style scoped>

.avatar{
    padding: 2.2px;
}

.barra{
    background: rgb(0,0,0);
background: linear-gradient(90deg, rgba(0,0,0,1) 0%, rgba(39,39,39,1) 100%);
}

#Btn{
    background: none;
}


</style>