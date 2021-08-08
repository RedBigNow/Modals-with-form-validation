<template>
    <div class="wrapper">
        <header>
            <div class="navbar">
                <div class="container">
                    <div class="navbar-content">
                        <div class="logo">MODALS</div>
                        <ul class="navbar-list">
                            <li class="navbar-item">
                                <a href="#" @click="modalAuth = !modalAuth">Authorization</a>
                            </li>
                            <li class="navbar-item">
                                <a href="#" @click="modalRegister = !modalRegister">Registration</a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </header>

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
                    <modalValidate 
                        v-if="modalValidate"
                        @close="modalValidate = false"/>

                    <!-- Modal auth -->
                    <modalAuth 
                        v-if="modalAuth"
                        @close="modalAuth = false"
                        @openRegister="modalAuth = false, modalRegister = true"/>

                    <!-- Modal register -->
                    <modalRegister
                        v-if="modalRegister"
                        @close="modalRegister = false"
                        @openAuth="modalRegister = false, modalAuth = true"/>

                </div>
            </section>
        </div>
    </div>
</template>

<script>
import modal from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'
import modalAuth from '@/components/ModalAuth.vue'
import modalRegister from '@/components/ModalRegister.vue'

export default {
    components: {
        modal, modalValidate, modalAuth, modalRegister
    },
    data () {
        return {
            modalFirst: false,
            modalSecond: {
                show: false,
                name: '',
                email: ''
            },
            modalValidate: false,
            modalAuth: false,
            modalRegister: false
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
.wrapper-content {
    padding-top: 80px;
}

.modal__btn-list {
    display: flex;
    justify-content: space-between;
}
</style>