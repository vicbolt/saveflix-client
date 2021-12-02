<template>
    <div class="sf-edit-movie">

        <SfEditMovie :movie="movie"/>

    </div>
</template>

<script>

export default ({
    async asyncData(ctx) {

        const paramsId = ctx.params.id
            
            const res = await fetch(`http://localhost:4500/api/movie/getOne/${paramsId}`)
            const data = await res.json()
            if(data.error){
                console.log(data.error)
            }

            return {
                movie: data.movie,
            }
    },
})

</script>