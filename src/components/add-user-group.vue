<template>
  <h2>Add User Group</h2>
  <div>
    <label for='title'>Title</label>
    <input type="text" v-model="title">

    <button @click="addUserGroup($event)">Submit</button>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)

export default {

  data () {
    return {
      title: ""
    }
  },

  methods: {
    addUserGroup (event) {
      this.$http.post('/perm/user_groups', {
        title: this.title
      }).then((resp) => {
        this.title = ''

        this.$dispatch('user-group-created', resp.data.data.user_group)
      }), (resp) => {
        alert(resp.data.message)
      }
    }
  }
}
</script>
