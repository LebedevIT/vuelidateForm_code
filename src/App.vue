<template>
  <transition mode="out-in" name="slide-left">
    <component :is="currentPage" @changePage="changePage"></component>
  </transition>
</template>

<script>
  import personal_form from './components/personal_form.vue'
  import address_form from './components/address_form.vue'
  import passport_form from './components/passport_form.vue'

  export default {
    components: {
      personal_form,
      address_form,
      passport_form
    },
    data() {
      return {
        currentPage: 'personal_form',
      }
    },
    methods: {
      changePage(page) {
        this.currentPage = page
      }
    },
  }
</script>

<style scoped lang="scss">
  @import 'assets/style.scss';
</style>