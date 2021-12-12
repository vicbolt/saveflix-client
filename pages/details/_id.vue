<template>
    <div class="sf-details">

        <SfDetails :post="post" />
        <SfComments :post="post" /> 

    </div>
</template>

<script>

export default ({

    async asyncData(ctx) {

        try{
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
            
            return res.json(error)
        }
    },

})
</script>


