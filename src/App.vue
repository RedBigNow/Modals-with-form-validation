<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">

          <div class="modal__btn-list">
            <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal with content</button>
            <button class="btn btnPrimary" @click="modalSecond.show = !modalSecond.show">Show modal with form</button>
            <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Show modal with form + validate</button>
          </div>
          
          <!-- first modal -->
          <modal
            title="First modal with content" 
            v-show="modalFirst"
            @close="modalFirst = false">
            <div slot="body">
              <p style="padding-bottom: 20px;">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
              <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Well done!</button>
            </div>
          </modal>

          <!-- second modal -->
          <modal
            title="Modal with form" 
            v-show="modalSecond.show"
            @close="modalSecond.show = false">
            <div slot="body">
              <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" required v-model="modalSecond.name">
                <label>Email:</label>
                <input type="email" required v-model="modalSecond.email">
                <button class="btn btnPrimary">Submit</button>
              </form>
            </div>
          </modal>

          <!-- Modal with validate -->
          <modalValidate v-if="modalValidate" @close="modalValidate = false"/>

        </div>
      </section>

    </div>
  </div>
</template>

<script>

import modal from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'

export default {
  components: {
    modal, modalValidate
  },
  data () {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: ''
      },
      modalValidate: false
    }
  },
  methods: {
    submitSecondForm () {
      console.log( {
        name: this.modalSecond.name,
        email: this.modalSecond.email
      })
      this.modalSecond.name = ''
      this.modalSecond.email = ''
      this.modalSecond.show = false
    }
  }
}
</script>

<style lang="scss">
  .modal__btn-list {
    display: flex;
    justify-content: space-between;
  }
</style>