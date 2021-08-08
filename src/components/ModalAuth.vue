<template>
    <modal
        title="Authorization"
        @close="$emit('close')">
        <div slot="body">
            <form @submit.prevent="onSubmit">
                <!-- Email -->
                <div class="form-item" :class="{ errorInput: $v.email.$error }">
                    <label>Email:</label>
                    <p class="errorText" v-if="!$v.email.required"> Field is required!</p>
                    <p class="errorText" v-if="!$v.email.email"> Email is not correct!</p>
                    <input
                        v-model="email"
                        :class="{ error: $v.email.$error }"
                        @change="$v.email.$touch()">
                </div>
                <!-- Password -->
                <div class="form-item" :class="{ errorInput: $v.password.$error }">
                    <label>Password:</label>
                    <p class="errorText" v-if="!$v.password.required"> Field is required!</p>
                    <p class="errorText" v-if="!$v.password.minLength"> Password must have at least {{ $v.password.$params.minLength.min }} letters</p>
                    <input
                        v-model="password"
                        :class="{ error: $v.password.$error }"
                        @change="$v.password.$touch()">
                </div>
                <!-- Button -->
                <button class="btn btnPrimary">Log in</button>
            </form>
            <div class="modal-bottom">
                <a class="another-modal" href="#" @click="openRegister">I need an account</a>
            </div>
        </div>
    </modal>
</template>

<script>
import { required, minLength, email } from 'vuelidate/lib/validators'
import modal from '@/components/UI/Modal.vue'

export default {
    components: {
        modal
    },
    data () {
        return {
            email: '',
            password: ''
        }
    },
    validations: {
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(8)
        }
    },
    methods: {
        onSubmit () {
            this.$v.$touch()
            if(!this.$v.$invalid) {
                const user = {
                    email: this.email,
                    password: this.password
                }
                console.log(user)

                //DONE
                this.email = ''
                this.password = ''
                this.$v.$reset()
                this.$emit('close')
            }
        },
        openRegister () {
            this.$emit('openRegister')
        }
    }
}
</script>

<style lang="scss">
.form-item .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 13.4px;
    color: red;
}

.form-item {
    &.errorInput .errorText {
        display: block;
    }
}

input.error {
    border-color: red;
}
</style>