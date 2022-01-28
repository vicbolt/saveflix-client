<template>
    <div class="sf-pendiente-details" v-if="visible">

        <SfPendienteDetails :post="post" />

    </div>
</template>

<script>

export default ({

    data(){
        return{
            visible: true
        }
    },

    async asyncData(ctx) {
        try{
            const id = ctx.params.id

            const config = require('/config')
            
            const res = await fetch(config.hostname + `api/moviePendiente/getOne/${id}`)
            const data = await res.json()
            
            if(data.movieP !== null){
                if(data.error){
                    console.log(data.error)
                }

                return {
                    post: data.movieP,
                }
            } else {

                const res2 = await fetch(config.hostname + `api/serialPendiente/getOne/${id}`)
                const data2 = await res2.json()
                
                if(data2.error){
                    console.log(data2.error)
                }

                return {
                    post: data2.serialP
                }
            }

        } catch(error){
            
            return console.log(error)
        }
    },

    beforeMount(){
        const token = localStorage.getItem("token")
            if(!token){
                this.visible = false
                return this.$router.push("/logIn")
            }
    }
})

</script>


