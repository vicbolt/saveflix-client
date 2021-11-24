<template>
    <div class="sf-series" v-if="visible">

        <SfMisSeries/>

    </div>
</template>

<script>
export default ({
    data() {
        return{
            visible: true
        }
    },

    beforeMount(){
        const token = localStorage.getItem('token')

        if(!token){
            this.visible = false
        }
    }
})
</script>