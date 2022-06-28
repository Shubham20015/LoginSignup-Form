<template>
<div class="center-div">
  <h2>Login</h2>
  <img src="./assets/logo.png" alt="company-logo" />
  <LoginUser v-if="isAdmin" />
  <LoginEmail v-else />
</div>
</template>

<script>
import LoginEmail from "./components/LoginEmail"
import LoginUser from "./components/LoginUser"

export default {
  name: 'App',
  components: {
    LoginEmail,
    LoginUser,
  },
  data(){
    return {
      isAdmin: false,
    }
  },
  provide() {
    return {
      adminLogin: this.adminLogin,
    }
  },
  methods: {
    adminLogin(){
      this.isAdmin = true;
    }
  }
}
</script>

<style>
*,
*::after,
*::before,
html {
margin: 0;
padding: 0;
box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  background: #13232c;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.center-div{
  width: 20rem;
  background: #1b303c;
  margin: 20px auto;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.center-div img{
  display: block;
  width: 8rem;
  margin: 12px auto;
}
</style>
