<template>
  <div class="user">
    <div class="avatar">
      <img src="{{ avatar }}" v-if="avatar">
    </div>
    <p class="nickname">
      {{ nickname }}
    </p>
    <slot name="action"></slot>
  </div>
</template>

<style lang="stylus">
.user
  width: 100px
  overflow: hidden

  .avatar
    text-align: center

    img
      max-width: 50px
      max-height: 50px
      border-radius: 50px
      border: solid 1px black

  .nickname
    margin: 0 auto
    text-align: center

  slot[name=action]
    margin: 0 auto
    text-align: center
</style>

<script>
export default {
  data () {
    return {
      avatar: "http://img6.douban.com/icon/u3770153-22.jpg",
      nickname: "soasme"
    }
  },

  props: ['avatar', 'nickname']
}
</script>
