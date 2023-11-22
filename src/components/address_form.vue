<template>
  <form @submit.prevent="submitForm">
    <article class="wrapper">
      <header>
        <p>Адрес</p>
        <span @click="backTo('personal_form')">Назад</span>
      </header>
      <article class="address__info">
        <section class="index">
          <div class="input">
            <label for="index">Индекс</label>
            <input type="number" placeholder="Введите индекс" id="index" v-model="index" />
          </div>
        </section>
        <section class="country">
          <div class="input">
            <label for="country">Страна</label>
            <input type="text" placeholder="Введите страну" id="country" v-model="country" />
          </div>
        </section>
        <section class="region">
          <div class="input">
            <label for="region">Регион</label>
            <input type="text" placeholder="Введите регион" id="region" v-model="region" />
          </div>
        </section>
        <section class="city">
          <div class="input">
            <label for="city">Город<span class="required">*</span></label>
            <input :class="{'error': v$.city.$errors.length > 0}" type="text" placeholder="Введите город" id="city" v-model="city" />
          </div>
        </section>
        <section class="street">
          <div class="input">
            <label for="street">Улица</label>
            <input type="text" placeholder="Введите улицу" id="street" v-model="street" />
          </div>
        </section>
        <section class="house">
          <div class="input">
            <label for="house">Дом</label>
            <input type="number" placeholder="Введите номер дом" id="house" v-model="house" />
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
        index: '',
        country: '',
        region: '',
        city: '',
        street: '',
        house: ''
      }
    },
    validations() {
      return {
        city: {
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
        this.$emit('changePage', 'passport_form')
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