<template>
    <h1>Admin</h1>
</template>

<script>
export default {
    data(){
        return {
            name:"Herry"
        }
    },
    beforeRouterEnter: (to, from, next) => {
        // alert("Hello" + this.name);
        // next();    
        next(vm =>{
            alert("Hello" + vm.name);
        })
        }
}
</script>
