<template>
<div class='users-form'>
    <div  v-for="(param, index) of params" :key='index'>
        <div v-if="param.type === 'required'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <input :name='param.name' class='users-form__text-component' :class="v$.formFields[param.name].$error ? 'users-form__is-invalid' : '' " v-model='formFields[param.name]'>
            <!--<users-Input class='text-component' :name='param.name' :class="v$.formFields[param.name].$error ? 'is-invalid' : '' " 
            v-model='formFields[param.name]'
            /> -->
            <span class='users-form__important'> * </span>
            <div v-if="v$.formFields[param.name].$error" class='users-form__error'>Ввод допустим только кириллицей.</div>
        </div>

        <div  v-else-if="param.type === 'date'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <input :name='param.name' class='users-form__text-component' :class="v$.formFields[param.name].$error ? 'users-form__is-invalid' : '' " v-model='formFields[param.name]'>
            <span class='users-form__important'> * </span>
            <div v-if="v$.formFields[param.name].$error" class='users-form__error'>Введите данные в формате  DD.MM.YYYY.</div>
        </div>

        <div v-else-if="param.type === 'input'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <input :name='param.name' class='users-form__text-component'>
        </div>

        <div v-else-if="param.type === 'phone'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <input :name='param.name' class='users-form__text-component' :class="v$.formFields[param.name].$error ? 'users-form__is-invalid' : '' " v-model='formFields[param.name]'>
            <span class='users-form__important'> * </span>
            <div v-if="v$.formFields.phone.$error" class='users-form__error'>Введите данные в формате  +7XXX-XXX-XX-XX.</div>
        </div>

        <div  v-else-if="param.type==='select'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <users-Select class='users-form__select-component' :name='param.name' :arr = 'param.arr' v-model='formFields[param.name]'
            :selected='setSelect(param.arr, formFields[param.name])'/>
        </div>

        <div  v-else-if="param.type==='document'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <users-Select class='users-form__select-component' :name='param.name' :class="v$.formFields.document.$error ? 'users-form__is-invalid' : '' " 
            :arr = 'param.arr' v-model='formFields[param.name]' :selected='setSelect(param.arr, formFields[param.name])'>
            </users-Select>
            <span class='users-form__important-for-slot'> * </span>
            <div v-if="v$.formFields.document.$error" class='users-form__error'>Укажите один из вариантов.</div>
        </div>

        <div v-else-if="param.type==='clientsGroup'">
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <users-Multiple class='users-form__select-component' :name='param.name' :class="v$.formFields.clientsCategory.$error ? 'users-form__is-invalid' : '' "
            :arr = 'param.arr' v-model='formFields.clientsCategory' :selected='setMultiple(param.arr, formFields[param.name])'>
            </users-Multiple>
            <span class='users-form__important-for-slot'> * </span>
            <div v-if="v$.formFields.clientsCategory.$error" class='users-form__error'>Укажите не менее одной категории.</div>
        </div>

        <div v-else-if='param.type === "messages"'>
            <label class='users-form__label' :for='param.name'> {{param.title}}: </label>
            <users-Checkbox  class='users-form__checkBox' :name='param.name' :id="param.name" 
            v-model='formFields.sendMessage'/>
            <span class='users-form__notification'> кликните, если хотите получать 
                                        уведомления <br> через смс
            </span>
        </div>
    </div>
    <div class='users-form__buttonDiv'>
        <button class='users-form__button' type='submit' @click='checkForm'>
            <span>Зарегистрироваться</span></button>
    </div>
    <div v-if='isHidden'>
        <modalWindow @closeModal='closeModal'/>
    </div>
</div>

</template>

<script>

//import usersInput from './UsersInput.vue'
import usersSelect from './UsersSelect.vue'
import usersMultiple from './UsersMultiple.vue'
import usersCheckbox from './UsersCheckbox.vue'
import useVuelidate from '@vuelidate/core'
import { required} from '@vuelidate/validators'
import modalWindow from './modalWindow.vue'

function alphabet(value) {
         let trimmed = value.trim();
        let re = /^[А-я`]+$/;
        return trimmed.match(re);
}

export default {
    setup () {
    return { v$: useVuelidate() }
  },
    name: 'usersData',
    components: {
        //usersInput,
        usersSelect,
        usersMultiple,
        usersCheckbox,
        modalWindow,
    }, 
    data() {
            return {
                    params: 
                    [
                        { title: 'Имя', type: 'required', name: 'firstName' },
                        { title: 'Фамилия', type: 'required', name: 'secondName' },
                        { title: 'Отчество', type: 'input', name: 'thirdName' },
                        { title: 'Дата рождения', type: 'date', name: 'birthDay' }, //регулярное + мм > 1 < 13
                        { title: 'Номер телефона', type: 'phone', name: 'phone' },
                        { title: 'Пол', type: 'select', name: 'gender', arr: 
                                                                                [{ value: 'female', label: 'Женский' }, { value: 'male', label: 'Мужской' }] },
                        { title: 'Группа клиентов', type: 'clientsGroup', name: 'clientsCategory', arr: 
                                                                                                    [{ label: 'VIP', value: 'vip' }, { label: 'Проблемные', value: 'problematic' },{ label: 'ОМС', value: 'OMS' }] }, //mandatory
                        { title: 'Лечащий врач', type: 'select', name: 'doctorName',  arr: [
                                                                                                { label: 'Невыбрано', value: 'default' },
                                                                                                { label: 'Иванов И.И.', value: 'Ivanov I.I' },
                                                                                                { label: 'Захарова З.З.', value: 'Zakharova Z.Z.' },
                                                                                                { label: 'Чернышева Ч.Ч.', value: 'Chenisheva Ch.Ch.' }], },
                        { title: 'Отправлять мне сообщения', type: 'messages', name: 'sendMessages', }, 
                        { title: "Индекс", type: 'input', name: 'index' },
                        { title: "Страна", type: 'input', name: 'country' },
                        { title: 'Область', type: 'input', name: 'area'  },
                        { title: 'Город', type: 'required', name: 'city' }, // required
                        { title: "Улица", type: 'input', name: 'street' },
                        { title: "Дом", type: 'input', name: 'building' },
                        { title: 'Тип документа', type: 'document', name: 'document', arr:[
                                                                                                { label: 'Паспорт', value: 'passport' },
                                                                                                { label: 'Водительское удостоверение', value: 'drive docs' },
                                                                                                { label: 'Свидетельство о рождение', value: 'birth docs' }
                                                                                            ], },
                        { title: 'Серия', type: 'input', name: 'series' },
                        { title: 'Номер', type: 'input', name: 'number' },
                        { title: 'Кем выдан', type: 'input', name: 'govState' },
                        { title: 'Дата выдачи', type: 'date', name: 'documentsDate' }, ],

                    formFields: {
                        firstName: "",
                        secondName:'',
                        birthDay: '',
                        phone: '+7',
                        clientsCategory: [],
                        document: '',
                        doctorName: 'null',
                        documentsDate: '',
                        city: '',
                        medic: '',
                        gender: 'null',
                        sendMessage: false,
                    },
                    selected: 'testtesttest',
                    isHidden: false,
                }
            },
            validations() {
                return {
                    formFields: {
                        firstName: {
                            required,
                            alphabet
                        },
                        secondName: {
                            required,
                            alphabet
                        },
                        birthDay: {
                            required,
                            checkPhoneNumber(value) {
                                let re = /^(\d{2}\.\d{2}\.)\d{4}$/;
                                return value.match(re);
                            },
                        },
                        phone: {
                            required,
                            checkPhoneNumber(value) {
                                let re = /^(\+?7\s?|\+?8\s?-?)9\d{2}\s?-?\d{3}\s?-?\d{2}\s?-?\d{2}$/;
                                return value.match(re);
                            }
                        },
                        clientsCategory: {
                            required,
                        },
                        document: {
                            required
                        },
                        documentsDate: {
                            required,
                            checkDocumentsDate(value) {
                            let re = /^(\d{2}\.\d{2}\.)\d{4}$/;
                            return value.match(re);
                            }
                        },
                        city: {
                            required,
                            alphabet
                        },
                    }
                }
            },
            methods: {
                setMultiple(arr, value) {
                    let optionalArr = [];
                    labelName: 
                    for(let elem2 of value) {
                        for(let elem1 of arr) {
                            if(elem1.value == elem2) {
                                optionalArr.push(elem1.label);
                                continue labelName;
                            } 
                        }
                    }
                    if(optionalArr.length == 0) {
                        return 'Выберите значение';
                    } else {
                        return optionalArr.join(', ');
                    }
                },

                setSelect(arr, value) {
                    let newArr = arr;
                    let elem = newArr.find(elem => elem.value === value );
                    if(Boolean(elem) == false) {
                        return {label: 'Выберите значение'};
                    } else {
                        return elem;
                    }
                },

                checkForm(e) {
                    e.preventDefault();
                    this.v$.formFields.$touch()
                    if(!this.v$.formFields.$error) {
                        console.log("Валидация прошла успешно")
                        this.isHidden = 'true';
                    } else {
                        console.log('Что-то неуказано')
                    }
                },
                closeModal() {
                    this.isHidden = false;
                }
            }
        }  
</script>

<style lang='sass' scoped>
.users-form
    width: 700px
    background-color: #ffffff
    border: solid 0.5px
    border-color: rgb(125, 191, 252)
    text-align: left
    box-shadow: 0 14px 28px rgba(158, 22, 22, 0.25), 0 10px 10px rgba(173, 18, 18, 0.22)
    justify-content: center 
    margin-left: auto
    margin-right: auto
    
.users-form__label 
    width: 100%
    display: inline-block
    margin-left: 2%
    margin-right: 3%
    padding-top: 2%
    padding-bottom: 2%
    font-size: 18px
    margin-left: 5%
    
    
.users-form__text-component 
    width: 80%
    height: 20px
    text-align: left
    font-size: 16px
    margin-bottom: 1%
    margin-left: 10%
    cursor: pointer
    
.users-form__select-component 
    margin-left: 10%
    height: 26px
    width: 80.5%
    margin-bottom: 10px
    
.users-form__checkBox 
    margin-left: 5%
    width: 24pх
    height: 24px
    
.users-form__buttonDiv 
    text-align: center
    
.users-form__button 
    width: 40%
    height: 80px
    background-color: rgb(255, 255, 255);
    font-size: 18px
    cursor: pointer
        text-align: center
        margin-bottom: 10px
        border: none;
        color: blueviolet
    
.users-form__button span:hover 
        color: #EF5A42
    
.users-form__error 
        width: 90%
        text-align: left
        font-size: 12px
        color: red
        margin-bottom: 1%
        margin-left: 5%
        border-radius: 10px
    
.users-form__is-invalid 
        border: solid red

.users-form__notification 
        font-size: 12px
        vertical-align: сenter
        display: inline-block;
        margin-left: 3px
        width: 300px
    
.users-form__important 
        color: red
        vertical-align: top
        display: inline-block
    
.users-form__important-for-slot 
        margin-left: 2px
        color: red
        vertical-align: top
        display: inline-block

@media screen and (max-width: 768px)
    .users-form 
            width: 100%
    .users-form__notification 
            width: 50%
    .users-form__button 
          width: 40%
          margin-top: 10px 
    .users-form__label 
            width: 60%
            display: block

@media screen and (max-width: 480px)
    .users-form 
            width: 100%
            margin: 0
    .users-form__button 
          width: 80%
          margin-top: 10px 
    .users-form__select-component 
            height: 40px    
    .users-form__buttonText 
            font-size: 14px
</style>