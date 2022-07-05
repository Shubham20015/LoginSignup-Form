<template>
<div class="center-div" v-if="isLoading">
  <h2 v-if="!isLoggedIn">Login</h2>
  <img src="./assets/logo.png" alt="company-logo" />
  <Homepage v-if="isLoggedIn" @logout="isLoggedIn = false" /> 
  <LoginUser v-else-if="isAdmin" @login="loggedIn"/>
  <LoginEmail v-else />
</div>
<div v-else>
  <img src="./assets/load.gif" alt="Loading..." />
</div>
</template>

<script>
import LoginEmail from "./components/LoginEmail"
import LoginUser from "./components/LoginUser"
import Homepage from "./components/Homepage"
import axios from "axios"
export default {
  name: 'App',
  components: {
    LoginEmail,
    LoginUser,
    Homepage,
  },
  mounted() {
    let data = JSON.parse(localStorage.getItem('data'));
    if(data && data.auth_token){
      this.checkLogin(data);
    }else{
      this.isLoading = true;
    }
  },
  data(){
    return {
      isAdmin: false,
      isLoggedIn: false,
      isLoading: false,
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
    },
    checkLogin(data) {
      const url = `https://wizapp.in/RestMirrorService/api/V1/ValidateToken`;
      const params = {
          cDeviceid: data.deviceId,
          cAuthToken: data.auth_token,
          GroupCode: data.GroupCode,
      }
      axios.get(url, {params})
      .then(() => {
          this.isLoggedIn = true;
          this.isLoading = true;
      })
    },
    loggedIn(){
      this.isAdmin = false;
      this.isLoggedIn = true;
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
