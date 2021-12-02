<template>
    <div class="sf-edit-movie">

        <SfEdit :post="post"/>

    </div>
</template>

<script>

export default ({
    async asyncData(ctx) {

        try{
            const paramsId = ctx.params.id
            
            const res = await fetch(`http://localhost:4500/api/movie/getOne/${paramsId}`)
            const data = await res.json()

            if(data.movie !== null){
                if(data.error){
                    console.log(data.error)
                }

                return {
                    post: data.movie,
                }
            } else {

                const res2 = await fetch(`http://localhost:4500/api/serial/getOne/${paramsId}`)
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