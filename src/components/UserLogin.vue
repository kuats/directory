<template>
  <div class="user-login">
    <h1 class="login-title">Авторизация</h1>

    <form @submit.prevent="submitForm" class="login-form">
      <label for="email" class="login-label">Email:</label>
      <input type="text" id="email" v-model="email" class="login-input" required>

      <label for="password" class="login-label">Пароль:</label>
      <input type="password" id="password" v-model="password" class="login-input" required>

      <button type="submit" class="login-button">Submit</button>

      <p v-if="loginError" class="login-error">{{ loginError }}</p>
    </form>

    <button v-if="isLoggedIn" @click="logout" class="logout-button fixed-button">Logout</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: '',
      isLoggedIn: false,
      loginError: ''
    };
  },
  methods: {
    submitForm() {
      const requestData = {
        email: this.email,
        password: this.password
      };

      axios
        .post('https://reqres.in/api/login', requestData)
        .then(response => {
          console.log(response.data.token);
          this.isLoggedIn = true;
          this.$emit('loginSuccess');
        })
        .catch(error => {
          console.error(error);
          this.loginError = 'Неправильный логин или пароль';
        });
    },
    logout() {
      this.isLoggedIn = false;
    }
  }
};
</script>

<style>
.user-login {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f0f0f0;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.login-title {
  margin-top: 0;
  margin-bottom: 20px;
  font-size: 24px;
  color: #333;
}

.login-form {
  margin-bottom: 20px;
}

.login-label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.login-input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}

.login-button {
  display: block;
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
}

.logout-button {
  position: absolute;
  top: 20px;
  right: 20px;
  padding: 10px;
  background-color: #f44336;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
}

.logout-button:hover,
.login-button:hover {
  opacity: 0.8;
}

.login-error {
  color: red;
  margin-top: 5px;
  font-size: 14px;
}
</style>
