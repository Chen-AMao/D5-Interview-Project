<template>
  <div id="app" class="app">
    <d5-header class="header"></d5-header>
    <div class="wrapper">
      <router-view class="router-view"/>
    </div>
    <d5-footer class="footer"></d5-footer>
  </div>
</template>

<script>
import D5Header from '@/components/D5Header.vue'
import D5Footer from '@/components/D5Footer.vue'

export default {
  name: 'App',
  components: {
    D5Header,
    D5Footer
  }
}

</script>

<style lang="stylus" scoped>
  .app
    position: absolute
    width: 100%
    height: 100%
    display: flex
    flex-direction: column
    .header
      height: 52px
      flex-grow: 0
      flex-shrink: 0
    .wrapper
      flex: 1
    .footer
      height: 60px
      flex-grow: 0
      flex-shrink: 0
</style>
