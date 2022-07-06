<template>
  <div class="info">
    <p>Group Code: {{adminInfo.GroupCode}}</p>
    <p>IP Address: {{adminInfo.ipAddress}}</p>
    <p>HO Location ID: {{adminInfo.locationId}}</p>
    <p>Location Alias: {{adminInfo.locationAlias}}</p>
  </div>
  <form @submit.prevent="submitform">
    <div class="container">
        <input type="text" name="username" id="username" placeholder=" " v-model="form.username" required/>
        <i class="fa-regular fa-user hover"></i>
        <label for="username">Username</label>
    </div>
    <div class="container">
        <input v-if="showPassword" type="text" name="password" id="password" placeholder=" " v-model="form.password" required/>
        <input v-else type="password" name="password" id="password" placeholder=" " v-model="form.password" required/>
        <i class="fa-solid fa-key hover"></i>
        <label for="password">Password</label>
        <i class="fa-regular right-0 hover" :class="{ 'fa-eye-slash': !showPassword, 'fa-eye': showPassword }" @click="toggleShow"></i>
    </div>
    <div class="container">
        <input type="text" name="location" id="location" placeholder=" " v-model="form.location" required/>
        <i class="fa-solid fa-location-dot hover"></i>
        <label for="location">Location Alias</label>
    </div>
    <span class="submitBtn">
        <button type="submit">Login</button>
    </span>
  </form>
</template>

<script>
import axios from "axios";
export default {
    name: 'LoginUser',
    emits: ["login"],
    data(){
        return {
            showPassword: false,
            form:{
                username: '',
                password: '',
                location: '',
                deviceId: '',
            },
            adminInfo:{
                GroupCode: '',
                ipAddress: '',
                locationId: '',
                locationAlias: '00',
            }
        }
    },
    mounted(){
        let data = localStorage.getItem("data");
        data = JSON.parse(data);
        this.adminInfo.GroupCode = data["GroupCode"];
        this.adminInfo.ipAddress = data["StaticIP"];
        this.adminInfo.locationId = data["HOLocationId"];
    },
    methods: {
        toggleShow(){
            this.showPassword = !this.showPassword;
        },
        async submitform(){
            if (!navigator.mediaDevices || !navigator.mediaDevices.enumerateDevices) {
                console.log("enumerateDevices() not supported.");
            }

            try{
                const devices = await navigator.mediaDevices.enumerateDevices();
                this.form.deviceId = devices[0].groupId;
            }catch(error){
                alert(error.name + ": " + error.message);
                return;
            }

            const url = `https://wizapp.in/RestMirrorService/api/V1/ValidateLogin`;
            const params = {
                cuser: this.form.username,
                cPassword: this.form.password,
                cDeviceid: this.form.deviceId.slice(0,15),
                GroupCode: this.adminInfo.GroupCode,
                LocationId: this.form.location,
            }
            axios.get(url, {params})
            .then((response) => {
                const data = response.data.result[0];
                localStorage.setItem("data",JSON.stringify({...this.adminInfo,auth_token: data.AUTHTOKEN,deviceId: data.DEVICEID}));
                this.$emit('login');
            })
            .catch((error) => {
                alert(error.message);
                this.form.username = '';
                this.form.password = '';
                this.form.location = '';
            });
        }
    }
}
</script>

<style scoped>
input:-webkit-autofill,
input:-webkit-autofill:hover, 
input:-webkit-autofill:focus, 
input:-webkit-autofill:active{
    box-shadow: 0 0 0 30px #1b303c inset !important;
    -webkit-text-fill-color: #fff !important;
}
.info{
    display: block;
    text-align: center;
    font-size: 14px;
    margin: 12px 0;
}
.info p{
    padding: 5px 0;
}
.container {
    position: relative;
    margin-bottom: 28px;
    color:#fff;
}
.container input:focus ~ label,
.container input:valid ~ label
{
    top: -5px;
    font-size: 0.7rem;
    font-weight: 600;
    color: #055bad;
}
.container input:focus ~ label::after,
.container input:valid ~ label::after{
    content: '';
}
i {
    position: absolute;
    top: 5px;
    left: 2px;
}
.right-0{
    left: 90%;
    cursor: pointer;
}
label{
    position: absolute;
    top: 5px;
    left: 30px;
    font-size: 0.9rem;
    transition: all 0.3s ease-in-out;
}
input{
    padding: 10px 30px;
    width: 100%;
    background: transparent;
    border: none;
    border-bottom: 2px solid;
    outline: none;
    color: #fff;
}
input:focus  ~ .hover, 
input:valid  ~ .hover, 
input:not(:placeholder-shown)  ~ .hover
{
    color: #055bad;
}
input:focus, 
input:valid, 
input:not(:placeholder-shown)
{
    border-color:#055bad;
}
.submitBtn{
    display: flex;
    justify-content: center;
}
button{
    margin-top: 16px;
    padding: 8px 50px;
    border-radius: 25px;
    cursor: pointer;
    border: none;
    font-size: 16px;
    font-weight: 600;
    text-align: center;
}
button:hover{
    background: #055bad;
    color: #fff;
}
</style>