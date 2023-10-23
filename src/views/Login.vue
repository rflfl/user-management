<template>
    <div>
        <h2>Login</h2>
        <hr>
        <div class="columns is-centered">
            <div class="column is-half">
                <div v-if="msgError" class="notification is-danger is-light">
                    <button @click="msgError = null" class="delete"></button>
                    {{ msgError }}
                </div>
                <div class="field">
                    <label class="label has-text-left">E-mail</label>
                    <div class="control">
                        <input type="email" name="email" placeholder="E-mail do usuário" class="input" v-model="email">
                    </div>
                </div>
                <div class="field">
                    <label class="label has-text-left">Senha</label>
                    <div class="control">
                        <input type="password" name="password" placeholder="Senha" class="input" v-model="password">
                    </div>
                </div>
                <hr>
                <button class="button is-success" @click="login">Acessar</button>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    data() {
        return {
            email: '',
            password: '',
            msgError: null,
        }
    },
    methods: {
        login() {
            axios.post("http://localhost:8686/login", {
                email: this.email,
                password: this.password
            }).then((res) => {
                localStorage.setItem('token', res.data.token)
                alert(res.data.token)
                this.$router.push({ name: 'home' });
            }).catch((err) => {
                    console.log(err.response.data.err);
                    this.msgError = err.response.data.err
            })
        }
    }
}

</script>
<style></style>