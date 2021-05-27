<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Registrarse</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Usuario</label>
                        <div class="control">
                            <input type="text" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Contraseña</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="field">
                        <label>Repita contraseña</label>
                        <div class="control">
                            <input type="password" class="input" v-model="repeat_password">
                        </div>
                    </div>
                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark">Registrarse</button>
                        </div>
                    </div>
                    <hr>

                    Si ya está registrado, <router-link to="/login"> acceda a su cuenta </router-link>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            password: '',
            repeat_password: '',
            errors: [],
        }
    },
    mounted() {
        document.title = 'Registrarse | DjanGoRide'
    },
    methods: {
        submitForm() {
            this.errors = []

            if (this.username === '') {
                this.errors.push('Ingrese su usuario')
            }

            if (this.password === '') {
                this.errors.push('Ingrese su contraseña')
            }

            if (this.password !== this.repeat_password) {
                this.errors.push('Las contraseñas no coinciden')
            }

            if (!this.errors.length) {
                const formData = {
                    username: this.username,
                    password: this.password
                }

                 axios
                    .post("/api/v1/users/", formData)
                    .then(response => {
                        toast({
                            message: 'Cuenta creada, puede acceder con su usuario!',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'bottom-right',
                        })

                        this.$router.push('/login')
                    })
                    .catch(error => {
                    if (error.response) {
                        for (const atribute in error.response.data) {
                            this.errors.push(`${atribute}: ${error.response.data[atribute]}`)
                        }

                        console.log(JSON.stringify(error.response.data))
                    } else if (error.message) {
                        this.errors.push('Ha ocurrido un error. Intente nuevamente')

                        console.log(JSON.stringify(error))
                    }
                })
            }

        },
    }
}
</script>