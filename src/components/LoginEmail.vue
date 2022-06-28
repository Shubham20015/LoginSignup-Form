<template>
  <form @submit.prevent="submitform">
    <div class="container">
        <input type="email" name="email" id="email" v-model="email" placeholder=" " required/>
        <i class="fa-regular fa-envelope"></i>
        <label for="email">Admin Email</label>
    </div>
    <span class="submitBtn">
        <button type="submit">Login</button>
    </span>
  </form>
</template>

<script>
import axios from "axios";
export default {
    name: 'LoginEmail',
    data(){
        return {
            email: '',
            data: [],
        }
    },
    inject: ['adminLogin'],
    methods: {
        submitform(){
            const url = `https://wizapp.in/MirrorServiceDetails/api/GetMirrorServiceDetails?cEmailAdd=${this.email}`;
            axios.get(url)
            .then((response) => {
                localStorage.setItem("data",JSON.stringify(response.data.datalist[0]));
                this.adminLogin();
            })
            .catch((error) => {
                console.log(error);
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
.container {
    position: relative;
}
.container input:focus ~ label,
.container input:valid ~ label,
.container input:not(:placeholder-shown) ~ label
{
    top: -10px;
    font-size: 0.7rem;
    font-weight: 600;
    color: #055bad;
}
.container input:focus ~ label::after,
.container input:valid ~ label::after,
.container input:not(:placeholder-shown) ~ label::after
{
    content: '';
}
i {
    position: absolute;
    top: 5px;
    left: 2px;
}
label{
    position: absolute;
    top: 5px;
    left: 30px;
    font-size: 0.9rem;
    transition: all 0.3s ease-in-out;
}
#email{
    padding: 10px 30px;
    width: 100%;
    background: transparent;
    border: none;
    border-bottom: 2px solid;
    outline: none;
    color: #fff;
}
#email:focus  + .fa-envelope, 
#email:valid  + .fa-envelope, 
#email:not(:placeholder-shown)  + .fa-envelope
{
    color: #055bad;
}
#email:focus, 
#email:valid, 
#email:not(:placeholder-shown)
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