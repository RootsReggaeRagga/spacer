<template>
    <div class="searchWrapper">
        <input name="search" id="search" :value="value" @input="handleChange" :class="{ dark }">
    </div>
</template>

<script>
    export default {
        name: "SearchInput",
        props: {
            value: {
                type: String,
                required: true,
            },
            dark: {
                type: Boolean,
                default: false
            }
        },
        methods: {
            handleChange(e) {
                this.$emit('input', e.target.value);
            },
        },
    }
</script>

<style lang="scss" scoped>

    .searchWrapper {
        margin-top: 60px;
        display: flex;
        flex-direction: column;
        width: 300px;

        input {
            text-align: center;
            font-size: 16px;
            background: none;
            height: 30px;
            color: #fff;
            border: 0;
            border-bottom: 1px solid white;
            transition: box-shadow .5s;
        }
        input:focus {
            outline: none;
            box-shadow: 0 10px 20px -8px rgba(255, 255, 255, .3)
        }
        .dark {
            color: #1e3d4a;
            border-bottom: 1px solid #1e3d4a;
            &:focus {
                box-shadow: 0 10px 20px -8px rgba(#1e3d4a, .3)
            }
        }
    }
</style>