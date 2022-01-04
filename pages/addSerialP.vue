<template>
    <div class="sf-add-serialP" v-if="visible">

        <SfAddSerialP/>

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