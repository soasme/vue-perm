<template>
  <div class="permission">
    <h3>{{ permission.title }}</h3>
    <h4>{{ permission.code }}</h4>
    <button @click="deletePermission">Delete Permission</button>
    <button @click="showAuthroizedUsers">Show Authorized Users</button>
    <div v-for="user in users">
      <user :avatar="user.avatar" :nickname="user.nickname">
        <button @click="removeUser(user)">Remove user</button>
      </user>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
import User from './user.vue'

Vue.use(VueResource)

export default {

  props: ['permission'],

  data () {
    return {
      title: "",
      code: "",
      users: []
    }
  },

  components: {
    User
  },

  methods: {

    showAuthroizedUsers (e) {
      this.$http.get(`/perm/permissions/${ this.permission.id}/users`).then((resp) => {
        this.$set('users', resp.data.users)
      })
    },

    deletePermission (e) {
      if (confirm('Do you really want to delete this permission?')) {
        this.$http.delete(`/perm/permissions/${ this.permission.id }`).then((resp) => {
          this.$dispatch('permission-deleted', this.permission)
        })
      }
    },

  }
}
</script>
