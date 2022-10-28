<template>
  <div class="home">
    <h1>这是主页哦，别走错了</h1>
<!--    <img alt="Vue logo" src="../assets/logo.png">-->
    <img alt="Vue logo" width="30%" src="../../public/images/001.jpg">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}
</script>