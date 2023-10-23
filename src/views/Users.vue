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
          <td><button class="button is-success">Editar</button> <button class="button is-danger">Deletar</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  created() {
    let req = {
      headers: {
        Authorization: "Baarer " + localStorage.getItem('token')
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
      users: []
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