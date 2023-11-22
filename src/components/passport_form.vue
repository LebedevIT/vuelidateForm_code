<template>
  <article class="passport_wrapper">

    <form @submit.prevent="submitForm">
      <article class="wrapper">
        <header>
          <p>Документ</p>
          <span @click="backTo('address_form')">Назад</span>
        </header>
        <article class="document__info">
          <section class="document_type">
            <div class="input">
              <label for="document_type">Тип документа<span class="required">*</span></label>
              <select :class="{'error': v$.document_type.$errors.length > 0}" id="document_type" v-model="document_type">
                <option value="" disabled selected>Выберите тип документа</option>
                <option v-for="elem in ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение']" :key="elem"
                  :value="elem">{{ elem }}</option>
              </select>
            </div>
          </section>
          <section class="serie">
            <div class="input">
              <label for="serie">Серия</label>
              <input type="number" placeholder="Введите серию" id="serie" v-model="serie" />
            </div>
          </section>
          <section class="number">
            <div class="input">
              <label for="number">Номер</label>
              <input type="number" placeholder="Введите номер" id="number" v-model="number" />
            </div>
          </section>
          <section class="given">
            <div class="input">
              <label for="given">Кем выдан</label>
              <input type="text" placeholder="Кем выдан" id="given" v-model="given" />
            </div>
          </section>
          <section class="given_date">
            <div class="input">
              <label for="given_date">Дата выдачи<span class="required">*</span></label>
              <input :class="{'error': v$.given_date.$errors.length > 0}" type="date" id="given_date" v-model="given_date" />
            </div>
          </section>
        </article>
      </article>
      <button class="button" type="submit">Отправить</button>
    </form>
      <article v-if="isModalShow" class="modal">
        <section class="content">
          <h1>Ура!</h1>
          <p>Поздравляем, форма отправлена!</p>
          <span @click="backTo('personal_form')">Назад</span>
        </section>
      </article>
  </article>
</template>

<script>
  import {
    useVuelidate
  } from '@vuelidate/core'
  import {
    required,
    helpers
  } from '@vuelidate/validators'


  export default {
    setup() {
      return {
        v$: useVuelidate()
      }
    },
    data() {
      return {
        document_type: '',
        serie: '',
        number: '',
        given: '',
        given_date: '',
        isModalShow: false
      }
    },
    validations() {
      return {
        document_type: {
          required: helpers.withMessage('Это поле обязательно для заполнения.', required)
        },
        given_date: {
          required: helpers.withMessage('Это поле обязательно для заполнения.', required)
        }
      }
    },
    methods: {
      submitForm() {
        this.v$.$touch();
        if (this.v$.$invalid) {
          return;
        }
        const button = document.querySelector('.button')
        button.textContent = "Успешно!"
        this.isModalShow = true
      },
      backTo(to) {
        this.$emit('changePage', to)
      }
    },
  }
</script>

<style scoped lang="scss">
 @import '../assets/style.scss';
</style>