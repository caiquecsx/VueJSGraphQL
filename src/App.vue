<template>
  <div id="app">
    <ul v-if="users && users.length">
      <li v-for="user of users" v-bind:key="user.id">
        {{user.email}}
      </li>
    </ul>
    <button @click="postUser()">Add User</button>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  name: "app",
  components: {},
  data() {
    return {
      users: [],
      errors: []
    };
  },
  created() {this.getUsers()},
  methods: {
    postUser() {
      axios
        .post("http://localhost:3000/api", {
          query: `
            mutation{
                createUser(input: {
                  name: "Caique"
                  email: "caique.c.s@outlook.com"
                  password: "12345"
                }) {
                id
                name
                email
                password
              }
            }
          `
        })
        .then(() => {
          this.getUsers();
        })
        .catch(err => {
          console.log(err)
        });
    },

    getUsers() {
      axios({
        url: "http://localhost:3000/api",
        method: "post",
        data: {
          query: `
      {
        getUsers{
          email,
          id
        }
      }
      `
        }
      }).then(result => {
        this.users = result.data.data.getUsers;
      });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
