<template>
  <h2>Add permission</h2>
  <div>
    <label for='title'>Title</label>
    <input type="text" v-model="title">

    <label for="code">Code</label>
    <input type="text" v-model="code">

    <button @click="addPermission($event)">Submit</button>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)

export default {

  methods: {
    addPermission (event) {
      this.$http.post('/perm/permissions', {
        title: this.title,
        code: this.code
      }).then((resp) => {

        this.title = ''
        this.code = ''

        this.$dispatch('permission-created', resp.data.data.permission)

      }, (resp) => {
        alert(resp.data.message)
      })
    }
  }

}
</script>
