<template>
    <div class="sf-edit-pendiente">

        <SfEditPendiente :post="post"/>

    </div>
</template>

<script>

export default ({
    async asyncData(ctx) {

        try{
            const paramsId = ctx.params.id
            const config = require('/config')
            
            const res = await fetch(config.hostname + `api/moviePendiente/getOne/${paramsId}`)
            const data = await res.json()

            if(data.movieP !== null){
                if(data.error){
                    console.log(data.error)
                }

                return {
                    post: data.movieP,
                }

            } else {

                const res1 = await fetch(config.hostname + `api/serialPendiente/getOne/${paramsId}`)
                const data1 = await res1.json()
                if(data1.error){
                    console.log(data1.error)
                }

                console.log("hola")

                return {
                    post: data1.serialP
                }
            }

        } catch(error){
            return console.log(error)
        }
    },

})

</script>