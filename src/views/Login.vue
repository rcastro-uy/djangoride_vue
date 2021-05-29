<template>
    <div class="page-log-in">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Acceder</h1>

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

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-green">Iniciar sesión</button>
                        </div>
                    </div>
                    <hr>

                    Primera vez? <router-link to="/sign-up"> Registrarse!</router-link>  O inicie sesión con Strava:
                </form>
                <br>
                <form @submit.prevent="loginStrava">
                    <div class="field">
                        <div class="control">
                            <button class="button is-black has-background-danger">Iniciar sesión con Strava</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
import Pizzly from 'pizzly-js'

export default {
    name: 'Login',
    data() {
        return {
            username: '',
            password: '',
            errors: [],
            userdata:[],
        }
    },
    mounted() {
        document.title = 'Login | DjanGoRide'
    },
    methods: {
        async submitForm() {
            axios.defaults.headers.common['Authorization'] = ""

            localStorage.removeItem["token"]

            const formData = {
                username: this.username,
                password: this.password
            }

            await axios
                .post("/api/v1/token/login", formData)
                .then(response => {
                    const token = response.data.auth_token

                    this.$store.commit('setToken', token)

                    axios.defaults.headers.common['Authorization'] = "Token " + token

                    // Para no perder el token al hacer refresh
                    localStorage.setItem("token", token)

                    const toPath = this.$route.query.to || '/cart'

                    this.$router.push(toPath)
                })
                .catch(error => {
                    if (error.response) {
                        for (const attribute in error.response.data) {
                            this.errors.push(`${attribute}: ${error.response.data[attribute]}`)
                        }
                    } else {
                        this.errors.push('Ha ocurrido un error. Intente nuevamente')

                        console.log(JSON.stringify(error))
                    }
                })
        },
        loginStrava(){
            const pizzly = new Pizzly({ host: 'djangoride.herokuapp.com'}) // Initialize Pizzly
            const myAPI = pizzly.integration('strava') // Replace with the API slugname
            let id = ''
            myAPI
            .connect()
            .then(({ authId }) => this.connectSuccess(myAPI, authId))
            //.then(this.connectSuccess( { authId } ))
            .catch(this.connectError);

            
        },
        connectSuccess(myAPI, data){
            console.log('Sucessfully connected!', data)
            myAPI
            .auth(data) // Replace with the authId previously obtained
            .get('/athlete') // Replace with a valid endpoint of the API
            .then( response => {
                return response.json()
            }).then(data => {
                this.userdata.push(data)
                console.log(data)
            } )
            .catch(console.error)
        }
    }
}
</script>

