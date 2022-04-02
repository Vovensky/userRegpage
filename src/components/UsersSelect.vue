<template>
    <!-- <select class='select' :name='name' :value='modelValue' @input="$emit('update:modelValue', $event.target.value)">
        <option value='' disabled >Выберите из списка</option>
        <option v-for='(elem, index) of arr' :key='index' :value = 'elem.value'> {{elem.label}} </option>
    </select> -->
    <div class='users-select'>
        <div class='users-select__title' @click='areOptionsVisible=!areOptionsVisible'>{{selected.label}}</div>
        <div class='users-select__options' v-if='areOptionsVisible' @mouseleave='areOptionsVisible=!areOptionsVisible'>
            <p v-for='(elem, index) of arr' :key='index' :value = 'elem.value' @click='selectOption(elem)'> 
                {{elem.label}} 
            </p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'usersSelect',
    model: {
        prop: 'value',
        event: 'select',
    },
    props: {
        name: String,
        modelValue: String,
        arr: {
            type: Array,
            default() {
                return [];
            },
        },
        selected: {
            String,
            default: '', 
            },
    },
    data() {
            return {
                areOptionsVisible: false,
            } 
    },
    methods: {
        selectOption(elem) {
            this.$emit('update:modelValue', elem.value);
            this.areOptionsVisible=false;
        }
    }
}
</script>

<style lang='scss' scoped>
    .users-select {
        position: relative;
        font-size: 16px;
        margin-bottom: 1%;
        cursor: pointer;
        display: inline-block;
        }
    .users-select p {
        margin: 1px;
    };
    .users-select__title {
        border: solid 0.5px;
        border-color: rgb(145, 144, 144);
        border-radius: 2px;
        width: 100%;
        height: 26px;
        text-align: center;
        display: inline-block;
    }
    .users-select__title::after {
        content: "↓";
        float: right;
        margin-right: 5px;

    }
    .users-select__options {
        border: solid 1px;
        position: absolute;
        width: 100%;
        background-color:#e7e7e7;
        border-radius: 2px;
        background-color: rgba(253, 253, 253, 0.9);
        z-index: 999;
    };
    .users-select__options p:hover{
        background: #befcfc
    }

    @media (max-width: 480px) {
        .users-select__title {
            height: 40px;
        }
    }

</style>