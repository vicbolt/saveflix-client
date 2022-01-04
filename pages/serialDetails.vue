<template>
    <div class="sf-serial-details" v-if="visible">

        <SfSerialDetails/>


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