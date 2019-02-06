<template>
    <div class="item" :style="style"></div>
</template>

<script>
    export default {
        name: "Item",
        props:{
            item:{
                type: Object,
                required: true
            },
        },
        data(){
            return{
                photo: this.item.links[0].href,
                title: this.item.data[0].title,
            }
        },
        computed:{
            style(){
                return `background-image:url("${this.photo}")`;
            }
        }
    }
</script>

<style scoped lang="scss">
.item{
    cursor:pointer;
    background-size:cover;
    background-repeat:no-repeat;
    min-width:40vw;
    height:250px;
    transition:.4s;
    @media (min-width:768px){
        min-width:25vw
    }
}
</style>