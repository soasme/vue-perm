<template>
  <button @click="toggleManageUserPermissions">Manage User Permissions</button>

  <div v-if="isLoading()">
    Loading
  </div>

  <ul v-if="isLoaded()">
    <li v-for="user in users">
      <label>
        <input type="checkbox"
          value="{{ user.id }}"
          v-model="authorizedUserIDs"
          @click="togglePermissionToUser(permission, user)">
        {{ user.nickname }}
      </label>
    </li>
  </ul>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)

export default {

  props: ['users', 'permission'],

  data () {
    return {
      authorizedUserIDs: [],
      users: [],
      manageUserPermissions: 'unloaded',
    }
  },

  methods: {

    isUnloaded() {
      return this.manageUserPermissions === 'unloaded'
    },

    isLoading() {
      return this.manageUserPermissions === 'loading'
    },

    isLoaded() {
      return this.manageUserPermissions === 'loaded'
    },

    toggleManageUserPermissions () {
      if (this.isLoaded()) {
        this.manageUserPermissions = 'unloaded'
      } else {
        this.manageUserPermissions = 'loading'

        console.log(this.manageUserPermissions)
        this.$http.get(
          `/perm/permissions/${ this.permission.id }/users`
        ).then((resp) => {
          this.$set('authorizedUserIDs', resp.data.data.users.map((user) => {
            return user.id
          }))
          this.manageUserPermissions = 'loaded'
        }, (resp) => {
          console.error('load user group data failed', this.permission, this.users)
        })
      }
    },

    togglePermissionToUser (permission, user) {
      if (this.authorizedUserIDs.indexOf(user.id) !== -1) {
        this.$http.delete(
          `/perm/permissions/${ permission.id }/users/${ user.id }`
        ).then((resp) => {
          this.$dispatch('user-permission-authorized', {
            permission: permission,
            user, user
          }, (resp) => {
            alert('Cancel failed')
          })
        })
      } else {
        this.$http.put(
          `/perm/permissions/${ permission.id }/users/${ user.id }`
        ).then((resp) => {
          this.$dispatch('user-permission-authorized', {
            permission: permission,
            user, user
          })
        }, (resp) => {
          alert('Auth failed')
        })
      }
    }
  }
}
</script>

