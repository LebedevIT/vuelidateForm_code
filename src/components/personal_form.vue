<template>
    <form @submit.prevent="submitForm">
      <article class="wrapper">
        <header>
          <p>Персональная информация</p>
        </header>
        <article class="personal__info">
          <section class="surname">
            <div class="input">
              <label for="surname">Фамилия<span class="required">*</span></label>
              <input :class="{'error': v$.surname.$errors.length > 0}" type="text" placeholder="Введите фамилию" id="surname" v-model="surname" />
            </div>
          </section>
          <section class="name">
            <div class="input">
              <label for="name">Имя<span class="required">*</span></label>
              <input :class="{'error': v$.name.$errors.length > 0}" type="text" placeholder="Введите имя" id="name" v-model="name" />
            </div>
          </section>
          <section class="patronymic">
            <div class="input">
              <label for="patronymic">Отчество</label>
              <input type="text" placeholder="Введите отчество" id="patronymic" v-model="patronymic" />
            </div>
          </section>
          <section class="bd_date">
            <div class="input">
              <label for="bd_date">Дата рождения<span class="required">*</span></label>
              <input :class="{'error': v$.bd_date.$errors.length > 0}" type="date" id="bd_date" v-model="bd_date" max="2007-12-31" />
            </div>
          </section>
          <section class="phone_number">
            <div class="input">
              <label for="phone_number">Номер телефона<span class="required">*</span></label>
              <input :class="{'error': v$.phone_number.$errors.length > 0}" type="text" placeholder="Формат: 79622601234" id="phone_number" v-model="phone_number" />
            </div>
          </section>
          <section class="sex">
            <div class="input">
              <label for="sex">Пол</label>
              <article class="genders">
                <section class="male">
                  <p>Мужчина:</p>
                  <input type="radio" name="sex" value="male" id="sex" v-model="sex" />
                </section>
                <section class="female">
                  <p>Женщина:</p>
                  <input type="radio" name="sex" value="female" id="sex" v-model="sex" />
                </section>
              </article>
            </div>
          </section>
          <section class="clients_group">
            <div class="input">
              <label for="clients_group">Группа клиентов<span class="required">*</span></label>
              <section class="select_block">
                <select :class="{'error': v$.clients_output.$errors.length > 0}" id="clients_group" v-if="!clients_group.length <= 0" v-model="clients_output"
                  @change="handleChangeSelect">
                  <option value="" disabled selected>Выберите группу клиентов</option>
                  <option v-for="elem in clients_group" :key="elem" :value="elem.name" :disabled="elem.isSelected">
                    {{ elem.name }}</option>
                </select>
                <section class="selected_elems" v-if="selected_clients_group.length > 0">
                  <p>Выбрано:</p>
                  <a v-for="elem in selected_clients_group" title="Нажмите, чтобы удалить" :key="elem"
                    @click="deleteSelectedElem(elem)">{{ elem }}</a>
                </section>
              </section>
            </div>
          </section>
          <section class="attending_doctor">
            <div class="input">
              <label for="attending_doctor">Лечащий врач</label>
              <select id="attending_doctor" v-model="attending_doctor">
                <option value="" disabled selected>Выберите лечащего врача</option>
                <option v-for="elem in ['Иванов', 'Захаров', 'Чернышева']" :key="elem" :value="elem">{{ elem }}</option>
              </select>
            </div>
          </section>
          <section class="dont_send_sms">
            <div class="input">
              <input id="dont_send_sms" v-model="dont_send_sms" type="checkbox" />
              <label for="dont_send_sms">Не отправлять СМС</label>
            </div>
          </section>
        </article>
      </article>
    <button type="submit">Дальше</button>
    </form>
  </template>
  
  <script>
    import {
      useVuelidate
    } from '@vuelidate/core'
    import {
      required,
      maxLength,
      minLength,
      numeric,
      helpers
    } from '@vuelidate/validators'
  
    function startsWithSeven(value) {
      return value.startsWith('7')
    }
  
    export default {
      setup() {
        return {
          v$: useVuelidate()
        }
      },
      data() {
        return {
          currentPage: 'personal',
          name: '',
          surname: '',
          patronymic: '',
          bd_date: '',
          phone_number: '',
          sex: '',
          clients_group: [{
              name: 'VIP',
              isSelected: false
            },
            {
              name: 'ОМС',
              isSelected: false
            },
            {
              name: 'Проблемные',
              isSelected: false
            }
          ],
          selected_clients_group: [],
          clients_output: '',
          attending_doctor: '',
          dont_send_sms: false
        }
      },
      validations() {
        return {
          name: {
            required: helpers.withMessage('Это поле обязательно для заполнения.', required)
          },
          surname: {
            required: helpers.withMessage('Это поле обязательно для заполнения.', required)
          },
          bd_date: {
            required: helpers.withMessage('Это поле обязательно для заполнения', required)
          },
          phone_number: {
            startsWithSeven: helpers.withMessage('Это поле должно начинаться с цифры 7.', startsWithSeven),
            numeric: helpers.withMessage('Это поле должно содержать только цифры.', numeric),
            maxLength: helpers.withMessage('Количество символов должно быть 11.', maxLength(11)),
            minLength: helpers.withMessage('Количество символов должно быть 11.', minLength(11)),
            required: helpers.withMessage('Это поле обязательно для заполнения.', required)
          },
          clients_output: {
            required: helpers.withMessage('Это поле обязательно для заполнения.', required)
          }
        }
      },
      methods: {
        submitForm() {
          this.v$.$touch();
          if (this.v$.$invalid) {
            console.log('error');
            console.log(this.v$);
            return;
          }
          this.$emit('changePage', 'address_form')
        },
        handleChangeSelect(elem) {
          const item = elem.target.value
          this.clients_group.forEach((elem) => {
            if (elem.name == item) {
              elem.isSelected = true
            }
          })
          this.selected_clients_group.push(item)
        },
        deleteSelectedElem(elem) {
          this.selected_clients_group = this.selected_clients_group.filter(item => item !== elem)
          this.clients_group.forEach((item) => {
            if (item.name == elem) {
              item.isSelected = false
            }
          })
        }
      },
    }
  </script>
  
  <style scoped lang="scss">
    @import '../assets/style.scss';
  </style>