<template>
    <div>
        <h2>Registro de Usuários</h2>
        <hr>
        <div class="columns is-centered">
            <div class="column is-half">
                <div v-if="msgError" class="notification is-danger is-light">
                    <button @click="msgError = null" class="delete"></button>
                    {{ msgError }}
                </div>
                <div class="field">
                    <label class="label has-text-left">Nome completo</label>
                    <div class="control">
                        <input type="text" name="name" placeholder="Nome do usuário" class="input" v-model="name">
                    </div>
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
                <button class="button is-success" @click="register">Cadastrar</button>
            </div>
        </div>
    </div>
</template>
<script>
import { ref } from 'vue'
import axios from 'axios';

export default {
    data() {
        return {
            name: '',
            email: '',
            password: '',
            msgError:null,
        }
    },
    methods:{
        register(){
            axios.post("http://localhost:8686/user", {
                name: this.name,
                email: this.email,
                password: this.password
            }).then(res => {
                console.log(res)
                this.$router.push({ name: 'Home' });
            }).catch(err => {
                console.log(err.response.data.err);
                this.msgError = err.response.data.err
            })
        }
    }
}

</script>
<style></style>