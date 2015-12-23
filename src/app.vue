<template>
 <h1>{{ msg }}</h1>
 <permission-list></permission-list>
</template>

<script>
import User from './components/user.vue'
import PermissionList from './components/permission-list.vue'

export default {
  data () {
    return {
      msg: 'Permission Manager'
    }
  },
  components: {
    User,
    PermissionList,
  }
}
</script>

<style lang="stylus">
</style>
