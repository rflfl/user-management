<template>
    <div>
        <h2>Editar Usuários</h2>
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
                <hr>
                <button class="button is-success" @click="update()">Editar</button>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    created() {
        let req = {
            headers: {
                Authorization: "Bearer " + localStorage.getItem('token')
            }
        }
        axios.get('http://localhost:8686/user/' + this.$route.params.id, req)
            .then((res) => {
                console.log(res)
                this.name = res.data.name
                this.email = res.data.email
                this.id = res.data.id
            })
            .catch((err) => {
                console.log(err)
                this.$router.push({ name: 'Users' })
            })
    },
    data() {
        return {
            name: '',
            email: '',
            id: -1,
            msgError: null,
        }
    },
    methods: {
        update() {
            let req = {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('token')
                }
            }
            axios.put("http://localhost:8686/user",{
                    id: this.id,
                    name: this.name,
                    email: this.email
                }, req).then(res => {
                    console.log(res)
                    this.$router.push({ name: 'Users' });
                }).catch(err => {
                    console.log(err.response.data.err);
                    this.msgError = err.response.data.err
                })
            }
        }
    }

</script>
<style></style>