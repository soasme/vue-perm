<template>
  <button @click="toggleManageUserGroupPermissions">Manage User Group Permissions</button>
  <div v-if="isLoading()">
    Loading
  </div>
  <ul v-if="isLoaded()">
    <li v-for="userGroup in userGroups">
      <label>
        <input type="checkbox"
          value="{{ userGroup.id }}"
          v-model="authorizedUserGroupIDs"
          @click="togglePermissionToUserGroup(permission, userGroup)">
        {{ userGroup.title }}
      </label>
    </li>
  </ul>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)

export default {

  props: ['userGroups', 'permission'],

  data () {
    return {
      authorizedUserGroupIDs: [],
      manageUserPermissions: 'unloaded',
    }
  },

  ready () {
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
    toggleManageUserGroupPermissions () {
      if (this.isLoaded()) {
        this.manageUserPermissions = 'unloaded'
      } else {
        this.manageUserPermissions = 'loading'
        this.$http.get(
          `/perm/permissions/${ this.permission.id }/user_groups`
        ).then((resp) => {
          this.$set('authorizedUserGroupIDs', resp.data.data.user_groups.map((group) => {
            return group.id
          }))
          this.manageUserPermissions = 'loaded'
        }, (resp) => {
          console.error('load user group data failed', this.permission, this.userGroups)
        })
      }
    },
    togglePermissionToUserGroup (permission, userGroup) {
      if (this.authorizedUserGroupIDs.indexOf(userGroup.id) !== -1) {
        this.$http.delete(
          `/perm/permissions/${ permission.id }/user_groups/${ userGroup.id }`
        ).then((resp) => {
          this.$dispatch('user-group-permission-authorized', {
            permission: permission,
            userGroup, userGroup
          }, (resp) => {
            alert('Cancel failed')
          })
        })
      } else {
        this.$http.put(
          `/perm/permissions/${ permission.id }/user_groups/${ userGroup.id }`
        ).then((resp) => {
          this.$dispatch('user-group-permission-authorized', {
            permission: permission,
            userGroup, userGroup
          })
        }, (resp) => {
          alert('Auth failed')
        })
      }
    }
  }
}
</script>
