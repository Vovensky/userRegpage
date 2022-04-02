<template>
  <div id='app'>
    <mainHeader/>
    <form name="users-info' class='form'" 
    method="post"
    novalidate="true">
        <userData>
        </userData>
    </form>
    <mainFooter/>
  </div>
</template>

<script>
import userData from './components/UsersRegPage.vue'
import mainHeader from './components/main-header.vue'
import mainFooter from './components/mainFooter.vue'
export default {
  name: 'app',
  components: {
      userData,
      mainHeader,
      mainFooter
  },
}
</script>

<style lang='scss' scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2e2d2d;
}

</style>