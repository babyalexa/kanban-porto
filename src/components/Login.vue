<template>
    <section class="section">
        <h1 class="title has-text-centered">
            Log in to your account
        </h1>
        <div class="columns is-mobile is-centered">
            <div class="column is-4">
                <form v-on:submit.prevent="submitForm">
                    <div class="field">
                        <label class="label">Email</label>
                        <div class="control has-icons-left has-icons-right">
                            <input
                                class="input"
                                type="email"
                                v-model="email"
                                placeholder="Email input"
                            />
                            <span class="icon is-small is-left">
                                <i class="fas fa-envelope"></i>
                            </span>
                        </div>
                    </div>
                    <div class="field">
                        <label class="label">Password</label>
                        <div class="control has-icons-left has-icons-right">
                            <input
                                class="input"
                                v-model="password"
                                type="password"
                                placeholder="Password input"
                            />
                            <span class="icon is-small is-left">
                                <i class="fas fa-key"></i>
                            </span>
                        </div>
                    </div>

                    <div class="buttons">
                        <input
                            type="submit"
                            value="Login"
                            class="button is-primary is-fullwidth"
                        />

                        <a
                            class="button is-light is-fullwidth"
                            @click="$router.push({ path: '/register' })"
                        >
                            Sign up
                        </a>

                        <GoogleLogin
                            class="button is-danger is-fullwidth"
                            :params="params"
                            :onSuccess="onSuccess"
                            :onFailure="onFailure"
                            ><i class="mdi mdi-google"></i> &nbsp; Login with
                            Google</GoogleLogin
                        >
                    </div>
                </form>
            </div>
        </div>
    </section>
</template>

<script>
import GoogleLogin from 'vue-google-login'
export default {
    data: () => {
        return {
            email: null,
            password: null,
            params: {
                client_id:
                    '249183931894-1jutc455mahrotsme8v47pttjo430g0e.apps.googleusercontent.com'
            },
            // only needed if you want to render the button with the google ui
            renderParams: {
                width: 250,
                height: 50,
                longtitle: true
            }
        }
    },
    methods: {
        submitForm: async function() {
            const data = {
                email: this.email,
                password: this.password
            }
            this.$store
                .dispatch('login', data)
                .then(path => {
                    console.log(this)
                    this.$router.push({ path })
                })
                .catch(e => {
                    console.log(e)
                })
        },
        onSuccess(googleUser) {
            // console.log(googleUser)

            // This only gets the user information: id, name, imageUrl and email
            // console.log(googleUser.getBasicProfile())
            var id_token = googleUser.getAuthResponse().id_token

            this.$store
                .dispatch('loginWithGoogle', id_token)
                .then(path => {
                    this.$router.push({ path })
                })
                .catch(e => {
                    console.log(e)
                })
        },
        onFailure(err) {
            console.log(err)
        }
    },
    components: {
        GoogleLogin
    }
}
</script>
