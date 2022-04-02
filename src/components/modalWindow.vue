<template>
    <div class='modalWindowStyle'>
        <div>
            <span class='text'>Пользователь зарегистрирован</span>
        </div>
        <button type='reset' @click='closeModal' class='button'> 
            <span>Далее</span>
        </button>
    </div>
</template>

<script>
export default {
    name: 'modalWindow',
    methods: {
        closeModal() {
            this.$emit('closeModal');
        }
    }
}
</script>

<style lang='sass' scoped>
.modalWindowStyle 
    display: block
    position: fixed
    top: 20%
    width: 700px
    height: 20%     
    border: solid 1px
    font-size: 20px
    background-color: white
    color: #016897

.text
    display: inline-block
    text-allign: center
    position: relative
    margin-left: 30%
    top: 100px
    
.button 
    width: 30%
    height: 20%
    top: 70%
    background-color: rgb(255, 255, 255)
    font-size: 20px
    border: solid 1px
    cursor: pointer
    text-align: center
    position: relative
    margin-left: 35%
    border: none
    color: blueviolet
    
.button span:hover
    color: #EF5A42

@media screen and (max-width : 768px)
    .modalWindowStyle
        width: 100%
    .button
        top: 50%;
@media screen and (max-width : 480px)
    .modalWindowStyle
        width: 100%
        height: 10%
    .button
        top: 50%
        font-size: 14px
        width: 50%
        margin-left: 25%
    .text
        display: block
        top: 50px
        font-size: 14px
        margin-left: 25%
        
</style>