<template>
    <modal
        title="Registration"
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
                <!-- Repeat password -->
                <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
                    <label>Repeat password:</label>
                    <p class="errorText" v-if="!$v.repeatPassword.required"> Field is required!</p>
                    <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword"> Passwords must be identical.</p>
                    <input
                        v-model="repeatPassword"
                        :class="{ error: $v.repeatPassword.$error }"
                        @change="$v.repeatPassword.$touch()">
                </div>
                <!-- Button -->
                <button class="btn btnPrimary">Register</button>
            </form>
            <div class="modal-bottom">
                <a class="another-modal" href="#" @click="openAuth">I already have an account</a>
            </div>
        </div>
    </modal>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
import modal from '@/components/UI/Modal.vue'

export default {
    components: {
        modal
    },
    data () {
        return {
            email: '',
            password: '',
            repeatPassword: ''
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
        },
        repeatPassword: {
            required,
            sameAsPassword: sameAs('password')
        }
    },
    methods: {
        onSubmit () {
            this.$v.$touch()
            if(!this.$v.$invalid) {
                const user = {
                    email: this.email,
                    password: this.password,
                    repeatPassword: this.repeatPassword
                }
                console.log(user)

                //DONE
                this.email = ''
                this.password = ''
                this.repeatPassword = ''
                this.$v.$reset()
                this.$emit('close')
            }
        },
        openAuth () {
            this.$emit('openAuth')
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