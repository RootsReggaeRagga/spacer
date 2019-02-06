<template>
    <div class="outerWrapper">
        <div class="innerWrapper">
            <div class="photo">
                <img :src="photo"/>
            </div>
            <div class="description">
                <h2 class="title">{{title}}</h2>
                <p>
                    {{description}}
                </p>
            </div>
        </div>
        <div class="close" @click="$emit('closeModal')"/>
    </div>
</template>

<script>
    export default {
        name: "Modal",
        props:{
           item:{
               type:Object,
               required:true,
           }
        },
        data(){
            return{
                photo: null,
                title: null,
                description: null,
            }
        },
        mounted(){
            this.photo =  this.item.links[0].href;
            this.title =  this.item.data[0].title;
            this.description =   this.item.data[0].description.substring(0, 200);
        }
    }
</script>

<style scoped lang="scss">
    .outerWrapper {
        background: #f6f6f6;
        max-width: 100%;
        height: 100%;
        position: fixed;
        top: 0;
        left: 0;
        @media (min-width:1024px){
            max-width:70%;
            height:60%;
            top:0;
            left:0;
            bottom:0;
            right:0;
            margin:auto;
            box-shadow: 0 30px 30px -10px rgba(0,0,0,0.3);
        }
    }

    .close {
        position: absolute;
        right: 0px;
        height: 30px;
        width: 30px;
        padding: 30px;
        top: 0px;
        cursor:pointer;
        &:after, &:before {
            position: absolute;
            content: "";
            top:30px;
            right:20px;
            width: 20px;
            height: 2px;
            background: black;
            display: block;
        }
        &:before {
            transform: rotate(45deg);
        }
        &:after {
            transform: rotate(-45deg);
        }
    }

    .innerWrapper {
        flex-direction: column;
        display: flex;
        height: 100%;
        padding: 50px;
        justify-content: center;
        align-items: center;
        @media (min-width:1024px){
            flex-direction: row;
            .photo{
                min-width:50%;
                margin-right:20px;
            }
        }
        .photo {
            width: 100%;
            height: auto;
            display: block;
            img {
                width: 100%;
            }
        }
        .description {
            color: #333;
        }
        .title{
            color:#1e3d4a;
        }
    }
</style>