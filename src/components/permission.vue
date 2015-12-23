<template>
  <div class="permission">
    <h3>{{ permission.title }}</h3>
    <h4>{{ permission.code }}</h4>
    <button @click="deletePermission">Delete Permission</button>
    <button @click="authorizePermissionToUser">Authorize Permission to User</button>
    <button @click="authorizePermissionToUserGroup">Authorize Permission to UserGroup</button>
    <button @click="showAuthroizedUsers">Show Authorized Users</button>
    <button @click="showAuthroizedUserGroups">Show Authorized UserGroups</button>
    <div v-for="user in users">
      <user :avatar="user.avatar" :nickname="user.nickname">
        <button @click="unauthroizeUserPermission(user)">
          Unauthorize User Permission
        </button>
      </user>
    </div>
    <div v-for="user_group in user_groups">
      <h5>{{ user_group.title }}</h5>
      <button @click="unauthroizeUserGroupPermission(user)">
        Unauthorize UserGroup Permission
      </button>
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
      users: [],
      user_groups: [],
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

    showAuthroizedUserGroups (e) {
    },

    deletePermission (e) {
      if (confirm('Do you really want to delete this permission?')) {
        this.$http.delete(`/perm/permissions/${ this.permission.id }`).then((resp) => {
          this.$dispatch('permission-deleted', this.permission)
        })
      }
    },

    authorizePermissionToUser (e) {
    },

    authorizePermissionToUserGroup (e) {
    },

  }
}
</script>
