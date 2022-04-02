<template>
    <!--<select :name='name' v-model='value' multiple >
        <option value='default' disabled>Укажите категорию клиента</option>
        <option v-for='(elem, index) of arr' :key='index' :value = 'elem.value'> {{elem.label}} </option>
    </select> -->
    <div class='users-select'>
        <div class='users-select__title' @click='areOptionsVisible=!areOptionsVisible'>{{selected}}</div>
        <div class='users-select__options' v-if='areOptionsVisible' @mouseleave='areOptionsVisible=!areOptionsVisible'>
            <div v-for='(elem, index) of arr' :key='index' :value = 'elem.value' @click='selectOption(elem)'> 
                {{elem.label}} 
            </div>
        </div>
    </div>

</template>

<script>
export default {
    name: 'usersMultiple',
    props: {
        modelValue: Array,
        name: String,
        arr: {
            type: Array,
        },
        selected: {
            String,
            default: '', 
            },
    },
    data() {
            return {
                areOptionsVisible: false,
                multipleArray: [],
            } 
    },
    //data() {
      //  return {
           // array: this.modalValue,
      //  }
    //},
    //computed: {
    //value: {
     // get() {
      //  return this.modelValue
     // },
      //set(value) {
     //   this.$emit('update:modelValue', value)
     // }
    //}}
   methods: {
        selectOption(elem) {
            if(this.multipleArray.includes(elem.value)) {
              let index = this.multipleArray.indexOf(elem.value);
              this.multipleArray.splice(index, 1);
            } else {
              this.multipleArray.push(elem.value);
            }
            this.$emit('update:modelValue', this.multipleArray);

        }
    }
}
</script>

<style lang='scss' scoped>
    .users-select {
        position: relative;
        width: 305px;
        font-size: 16px;
        margin-bottom: 1%;
        cursor: pointer;
        display: inline-block;
    };
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
        top: 30px;
        width: 100%;
        background-color:#e7e7e7;
        border-radius: 2px;
        text-align: center;
        background-color: rgba(253, 253, 253, 0.9);
        z-index: 999;
    };
    .users-select__options div:hover{
        background: #fdfafa
    }
    @media (max-width: 480px) {
        .users-select__title {
            height: 40px;
        }
    }
</style>