<template>
    <div class="sf-add-movieP" v-if="visible">
        <SfAddMovieP/>
    </div>
</template>


<script>

export default ({
    data() {
        return {
            visible: true
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
