<template>
        <input type='checkbox'  :name='name' v-model='value'>
</template>

<script>
export default {
    name: 'usersCheckbox',
    props: {
        name: String,
        modelValue: Boolean,
        },
    computed: {
    value: {
        get() {
            return this.modelValue
        },
        set(value) {
            this.$emit('update:modelValue', value)
        }
    }
  }
}
</script>

<style lang='sass' scoped>
input
    width: 24px;
    height: 24px;
</style>