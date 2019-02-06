<template>
    <div :class="[{flexStart: step === 1}, 'wrapper']">
        <transition name="slide">
            <div class="logo" v-if="step === 1">
                <h5>SPACER</h5>
            </div>
        </transition>
        <transition name="fade">
            <HeroImage v-if="step === 0"/>
        </transition>
        <Claim v-if="step === 0"/>
        <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1"/>
        <div class="results" v-if="results && !loading && step ===1">
            <Item v-for="item in results" :item="item" :key="item.data[0].nasa_id" @click.native="handleModalOpen(item)"/>

        </div>
        <div class="loader"  v-if="step === 1 && loading"></div>
        <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = false"/>
    </div>
</template>

<script>
    import axios from 'axios';
    import debounce from 'lodash.debounce';
    import Claim from '@/components/Claim.vue'
    import SearchInput from '@/components/SearchInput.vue'
    import HeroImage from "@/components/HeroImage.vue";
    import Item from "@/components/Item.vue";
    import Modal from "@/components/Modal.vue";

    const API = 'https://images-api.nasa.gov/search';

    export default {
        name: 'App',
        components: {Modal, Item, HeroImage, SearchInput, Claim},
        data() {
            //var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

            return {
                modalOpen: false,
                step: 0,
                modalItem: null,
                loading: false,
                searchValue: '',
                results: [],
            }
        },
        methods: {
            handleModalOpen(item){
              this.modalOpen = true;
              this.modalItem = item;
            },
            handleInput: debounce(function () {
                this.loading = true;

                console.log(this.searchValue);
                axios.get(`${API}?q=${this.searchValue}&media_type=image`)
                    .then((response) => {
                        console.log(response.data.collection.items);
                        this.results = response.data.collection.items;
                        this.loading = false;
                        this.step = 1;
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }, 600),
        },
    };
</script>


<style lang="scss">
    @import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&subset=latin-ext');

    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        padding: 0;
        font-family: 'Montserrat', sans-serif;
    }

    .wrapper {
        position: relative;
        width: 100%;
        height: 100vh;
        padding: 30px;
        display: flex;
        margin: 0;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        &.flexStart {
            justify-content: flex-start;
        }
    }

    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s;
    }

    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */
    {
        opacity: 0;
    }

    .slide-enter-active, .slide-leave-active {
        transition: margin-top .5s;
    }

    .slide-enter, .slide-leave-to /* .fade-leave-active below version 2.1.8 */
    {
        margin-top: -50px;
    }

    .logo {
        position: absolute;
        top: 0px;
        font-size: 30px;
        color: #1e3d4a;
    }

    .results {
        margin-top:50px;
        //width:80%;
        display:grid;
        grid-template-columns:1fr 1fr ;
        grid-gap:20px;
        @media (min-width:768px){
            grid-template-columns:1fr 1fr 1fr ;
        }
    }
    .loader {
        margin-top:100px;
        display: inline-block;
        width: 64px;
        height: 64px;

    }
    .loader:after {
        content: " ";
        display: block;
        width: 46px;
        height: 46px;
        margin: 1px;
        border-radius: 50%;
        border: 5px solid #1e3d4a;
        border-color: #1e3d4a transparent #1e3d4a transparent;
        animation: loading 1.2s linear infinite;
        @media (min-width:768px){
            width:90px;
            height:90px;
        }
    }
    @keyframes loading {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }


</style>
