<template>
  <add-permission 
    @permission-created="appendNewPermission"
    ></add-permission>
  <div v-for="permission in permissions">
    <permission :permission="permission"
      @permission-deleted="removePermission"
    ></permission>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
import Permission from './permission.vue'
import AddPermission from './add-permission.vue'
import User from './user.vue'

Vue.use(VueResource)

export default {
  data () {
    return {
      permissions: []
    }
  },

  components: {
    Permission,
    AddPermission,
  },

  ready () {
    this.$http.get('/perm/permissions').then((resp) => {
      this.$set('permissions', resp.data.data.permissions)
    })
  },

  methods: {
    appendNewPermission (permission) {
      this.permissions.splice(0, 0, permission)
    },
    removePermission (permission) {
      this.permissions.$remove(permission)
    }
  }


}
</script>

