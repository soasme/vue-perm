<template lang="jade">
div
  img(class="logo", src="./assets/logo.png")
  h1 {{msg}}
  comp-a
  comp-b
  counter
  search-bar
</template>

<script>
import CompA from './components/a.vue'
import CompB from './components/b.vue'
import Counter from './components/counter.vue'
import SearchBar from './components/search-bar.vue'

export default {
  data () {
    return {
      msg: 'Hello from vue-loader!'
    }
  },
  components: {
    CompA,
    CompB,
    Counter,
    SearchBar,
  }
}
</script>

<style lang="stylus">
font-stack = Helvetica, sans-serif
primary-color = #999
body
  font 100% font-stack
  color primary-color

.logo
  width 40px
  height 40px
</style>
