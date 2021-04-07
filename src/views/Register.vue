<template>
  <div class="register">
    <div class="left">
      <h1>Sign up</h1>
      <form @submit.prevent="registerUser">
        <p class="input-field">
          <input type="text" name="username" placeholder="Username" v-model.trim="state.username"/>
          <span v-if="v$.username.$error">{{ v$.username.$errors[0].$message }}</span>
        </p>
        <p class="input-field">
          <input type="text" name="email" placeholder="E-mail" v-model.trim="state.email"/>
          <span v-if="v$.email.$error">{{ v$.email.$errors[0].$message }}</span>
        </p>
        <p class="input-field">
          <input type="password" name="password" placeholder="Password" v-model="state.password.password"/>
          <span v-if="v$.password.password.$error">{{ v$.password.password.$errors[0].$message }}</span>
        </p>
        <p class="input-field">
          <input type="password" name="password2" placeholder="Retype password" v-model="state.password.confirm"/>
          <span v-if="v$.password.confirm.$error">{{ v$.password.confirm.$errors[0].$message }}</span>
        </p>
        <p class="input-field">
          <input type="text" name="name" placeholder="Firt Name" v-model.trim="state.firstName"/>
          <span v-if="v$.firstName.$error">{{ v$.firstName.$errors[0].$message }}</span>
        </p>
        <p class="input-field">
          <input type="text" name="name" placeholder="Last Name" v-model.trim="state.lastName"/>
          <span v-if="v$.lastName.$error">{{ v$.lastName.$errors[0].$message }}</span>
        </p>
        <p class="input-field">
          <input type="number" name="mobile" placeholder="Mobile Number" v-model.trim="state.mobileNumber"/>
          <span v-if="v$.mobileNumber.$error">{{ v$.mobileNumber.$errors[0].$message }}</span>
        </p>
        <input type="submit" name="signup_submit" value="Sign up" />

        <div class="status">
          {{ statusMessage }}
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, reactive } from 'vue';
import axios from 'axios';
import useValidate from '@vuelidate/core';
import { required, email, minLength, sameAs } from '@vuelidate/validators';

export default defineComponent({
  setup () {
    const state = reactive({
      username:"",
      email: "",
      password: {
        password: "",
        confirm: "",
      },
      firstName: "",
      lastName: "",
      mobileNumber: "",
    });

    const rules = computed(() => {
      return {
        username: { required, minLength: minLength(6) },
        email: { required, email },
        password: {
          password: { required, minLength: minLength(6) },
          confirm: { required, sameAs: sameAs(state.password.password)},
        },
        firstName: { required },
        lastName: { required },
        mobileNumber: { required },
      }
    });

    const v$ = useValidate(rules, state);

    return {
      state,
      v$,
    }
  },
  name: 'Register',
  data(){
    return {
      statusMessage: ""
    }
  },
  methods: {
    registerUser(){
      this.v$.$validate();
      if(this.v$.$error){
        this.statusMessage = "Fill the fields correctly";
        return;
      } 
      // axios.post("http://localhost:3000/api/v1/user/", 
      //   {
      //     username: state.username,
      //     password: this.password,
      //     email: this.email,
      //     firstName: this.firstName,
      //     lastName: this.lastName,
      //     mobileNumber: this.mobileNumber
      //   }
      // ).then((result) => {
      //   this.statusMessage = result.data.message;
      // });
      console.log(this.$data, this.state);
    }
  }
});
</script>

<style scoped>
body {
  margin: 0;
  padding: 0;
  background: #DDD;
  font-size: 16px;
  color: #222;
  font-family: 'Roboto', sans-serif;
  font-weight: 300;
}

.register {
  position: absolute;
  background: #FFF;
  border-radius: 2px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  padding: 1.5rem 5rem;
  text-align: center;
}

h1 {
  margin: 0 0 20px 0;
  font-weight: 300;
  font-size: 28px;
}

.input-field span {
  display: block;
  margin-top: -10px;
  font-size: 13px;
  text-align: left;
  color: red;
  margin-left: 0.5rem;
}

input[type="text"],
input[type="password"] {
  display: block;
  box-sizing: border-box;
  margin-bottom: 15px;
  padding: 4px;
  width: 220px;
  height: 32px;
  border: none;
  border-bottom: 1px solid #AAA;
  font-family: 'Roboto', sans-serif;
  font-weight: 400;
  font-size: 15px;
  transition: 0.2s ease;
}
input[type="number"]{
    display: block;
  box-sizing: border-box;
  margin-bottom: 15px;
  padding: 4px;
  width: 220px;
  height: 32px;
  border: none;
  border-bottom: 1px solid #AAA;
  font-family: 'Roboto', sans-serif;
  font-weight: 400;
  font-size: 15px;
  transition: 0.2s ease;
}

input[type="number"]:focus{
    border-bottom: 2px solid #16a085;
  color: #16a085;
  transition: 0.2s ease;
}


input[type="text"]:focus,
input[type="password"]:focus {
  border-bottom: 2px solid #16a085;
  color: #16a085;
  transition: 0.2s ease;
}

input[type="submit"] {
  margin-top: 20px;
  width: 120px;
  height: 32px;
  background: #16a085;
  border: none;
  border-radius: 2px;
  color: #FFF;
  font-family: 'Roboto', sans-serif;
  font-weight: 500;
  text-transform: uppercase;
  transition: 0.1s ease;
  cursor: pointer;
}

input[type="submit"]:hover,
input[type="submit"]:focus {
  opacity: 0.8;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
  transition: 0.1s ease;
}

input[type="submit"]:active {
  opacity: 1;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.4);
  transition: 0.1s ease;
}

.status {
  margin-top: 20px;
}
</style>