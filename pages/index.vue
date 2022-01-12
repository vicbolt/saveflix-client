<template>
    <div id="example-1">
        <transition name="slide-fade">
            <h1 v-if="show"> S A V E F L I X</h1>
        </transition>
    </div>
</template>

<script>

export default ({
    layout: 'auth',
    
    data() {
        return{
            show: false,
        }     
    },

    mounted(){
        setTimeout(()=>{
            this.show = true
        }, 1500)

        setTimeout(()=>{
            this.show = false
        }, 4500)

        setTimeout(()=>{
            const token = localStorage.getItem('token')

            const strId = localStorage.getItem('userId')
            const id = JSON.parse(strId)

            if(token){
                this.$router.push(`/explorar/${id}`)
            } else {
                this.$router.push('/login')
            }
            
        }, 6500)
        
    }
})
</script>


<style scoped>

h1{
    margin-top: 20%;
    text-align: center;
    color: rgb(229,9,20);
    font-size: 700%;
    font-family: 'Bebas Neue';
    font-weight: 100;
}


.slide-fade-enter-active {
  transition: all .300px ;
  opacity: 1;
}

.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0) ;
  opacity: 0;
}
.slide-fade-enter, .slide-fade-leave-to{
  transform: translateX(10px);
  opacity: 0;
}

</style>