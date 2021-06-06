<template>
  <div class="container">
    <div class="row justify-content-center align-item-center text-center">
      <div class="col-12 col-md-10 col-lg-8">
        <h3>Child app 3</h3>
        <p>This child application aims to show you how user authentication can be done</p>
        <br/>
        <br/>
        <h2>
          Currently user is <span class="pink-text" v-if="!isLoggedIn">not</span> logged in
        </h2>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      sticherAppDomain: process.env.VUE_APP_STITCHER_DOMAIN,
      isLoggedIn: localStorage.getItem('jwt')
    }
  },
  methods: {
    listenForStitcherMessage(event) {
      if (this.sticherAppDomain && this.sticherAppDomain === event.origin) {
        // check if the message format is valid
        if ( event.data && event.data.action) {
          if (event.data.action === 'logout') {
            this.logout()
          } else if (event.data.action === 'login' && event.data.token) {
            this.login(event.data.token)
          }
        }
      }
    },
    login(token) {
      this.isLoggedIn = true
      localStorage.setItem('jwt', token)
    },
    logout() {
      this.isLoggedIn = false
      localStorage.removeItem('jwt')
    },
  },
  mounted() {
    window.addEventListener('message', this.listenForStitcherMessage)
  },
  beforeUnmount () {
    window.removeEventListener('message', this.listenForStitcherMessage)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 60px;
  background-color: azure;
  height: 100vh;
}
.pink-text {
  color: #FF4083;
}
</style>
