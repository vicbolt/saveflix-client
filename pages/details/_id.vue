<template>
    <div class="sf-details" v-if="visible">

        <SfDetails :post="post" />
        <SfComments :post="post" /> 

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
            const token = localStorage.getItem("token")
            if(!token){
                this.visible = false
                return this.$router.push("/logIn")
            }
            const paramsId = ctx.params.id

            const config = require('/config')
            
            const res = await fetch(config.hostname + `api/movie/getOne/${paramsId}`)
            const data = await res.json()
            
            if(data.movie !== null){
                if(data.error){
                    console.log(data.error)
                }

                return {
                    post: data.movie,
                }
            } else {

                const res2 = await fetch(config.hostname + `api/serial/getOne/${paramsId}`)
                const data2 = await res2.json()
                
                if(data2.error){
                    console.log(data2.error)
                }

                return {
                    post: data2.serial
                }
            }

        } catch(error){
            
            return console.log(error)
        }
    },

})
</script>


