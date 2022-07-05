<template>
  <div>
    <h2>Welcome to wizapp</h2>
    <button class="btn" @click="logout">Logout</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
    name: 'Homepage',
    methods : {
      logout() {
        const url = `https://wizapp.in/RestMirrorService/api/V1/DestroyToken`;
        let data = JSON.parse(localStorage.getItem('data'));
        const params = {
          cDeviceid: data.deviceId,
          cAuthToken: data.auth_token,
          GroupCode: data.GroupCode,
        }
        axios.get(url, {params})
        .then(() => {
          localStorage.removeItem("data");
          this.$emit('logout');
        })
      }
    }
}
</script>

<style scoped>
  div {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .btn{
    padding: 10px 25px;
    margin: 10px 0;
    border-radius: 25px;
    border: none;
    background: cadetblue;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
    font-weight: 600;
    cursor: pointer;
  }
  .btn:hover{
    box-shadow: none;
  }
</style>