<template>
    <div class="sf-details" v-if="visible">

        <SfDetails/>
        <SfComments /> 

    </div>
</template>

<script>
export default ({

    data(){
        return{
            visible: true
        }
    },

    beforeMount(){
    const token = localStorage.getItem('token')
        if(!token){
            this.visible = false
            return this.$router.push("/logIn")

        }
    },
    
    beforeDestroy(){
        localStorage.removeItem("postId")
    },
})
</script>


