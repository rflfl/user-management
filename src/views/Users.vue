<template>
  <div>
    <h1>Painel Administrativo</h1>
    <hr>
    <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Email</th>
          <th><abbr title="Role">Role</abbr></th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.role == 1 ? 'Administrador' : 'usuario' }}</td>
          <td>
            <router-link :to="{name: 'Edit', params:{id:user.id}}">
              <button class="button is-success">Editar</button>
            </router-link>            <button @click="showDelModal(user.id)" class="button is-danger">Deletar</button></td>
        </tr>
      </tbody>
    </table>
    <div :class="{ modal: true, 'is-active': showModal }">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Excluir registro</p>
          <button class="delete" @click="showModal = !showModal" aria-label="close"></button>
        </header>
        <section class="modal-card-body">
          <p>Você deseja excluir este usuário?</p>
        </section>
        <footer class="modal-card-foot">
          <button class="button" @click="showModal = !showModal">Cancel</button>
          <button class="button is-success" @click="deleteUser()">Sim, excluir usuário!</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  created() {
    let req = {
      headers: {
        Authorization: "Bearer " + localStorage.getItem('token')
      }
    }
    axios.get("http://localhost:8686/user", req)
      .then((res) => {
        console.log(res)
        this.users = res.data
      })
      .catch((err) => {
        console.log(err)
      })
  },
  data() {
    return {
      users: [],
      showModal: false,
      deleteUserId: -1
    }
  },
  methods: {
    showDelModal(id) {
      this.showModal = true
      this.deleteUserId = id
    },
    deleteUser(id) {
      let req = {
        headers: {
          Authorization: "Baarer " + localStorage.getItem('token')
        }
      }
      axios.delete("http://localhost:8686/user/"+this.deleteUserId, req)
      .then((res) => {
        console.log(res)
        this.showModal = false;
        this.users = this.users.filter(u => u.id != this.deleteUserId)
      })
      .catch((err) => {
        console.log(err)
        this.showModal = false;
      })
    }
  },
  filters: {
    processRole: function (value) {
      if (value == 0) {
        return "Usuário"
      } else if (value == 1) {
        return "Admin"
      }
    }
  }
}
</script>

<style></style>