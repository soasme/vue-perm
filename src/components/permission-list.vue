<template>
  <add-permission 
    @permission-created="appendNewPermission"
    ></add-permission>
  <add-user-group
    @user-group-created="appendNewUserGroup"
  ></add-user-group>
  <div v-for="permission in permissions">
    <permission :permission="permission"
      @permission-deleted="removePermission"
    >
      <div slot="action">
        <authorize-permission-to-user-group
          :user-groups="userGroups"
          :permission="permission"
        ></authorize-permission-to-user-group>
      </div>
    </permission>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
import Permission from './permission.vue'
import AddPermission from './add-permission.vue'
import User from './user.vue'
import AddUserGroup from './add-user-group.vue'
import AuthorizePermissionToUserGroup from './authorize-permission-to-user-group.vue'

Vue.use(VueResource)

export default {
  data () {
    return {
      permissions: [],
      userGroups: [],
    }
  },

  components: {
    Permission,
    AddPermission,
    AddUserGroup,
    AuthorizePermissionToUserGroup,
  },

  ready () {
    this.$http.get('/perm/permissions').then((resp) => {
      this.$set('permissions', resp.data.data.permissions)
    }, (resp) => {
      alert('Load permission failed')
    })

    this.$http.get('/perm/user_groups').then((resp) => {
      this.$set('userGroups', resp.data.data.user_groups)
    }, (resp) => {
      alert('Load userGroups failed')
    })
  },

  methods: {
    appendNewPermission (permission) {
      this.permissions.splice(0, 0, permission)
    },
    removePermission (permission) {
      this.permissions.$remove(permission)
    },
    appendNewUserGroup (userGroup) {
      this.userGroups.splice(0, 0, userGroup)
    }
  }


}
</script>

