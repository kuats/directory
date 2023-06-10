<template>
    <div class="create-user">
      <form @submit.prevent="createUser" class="create-user-form">
        <div class="form-group">
          <label for="name">ФИО:</label>
          <input type="text" id="name" v-model="name" required>
        </div>
  
        <div class="form-group">
          <label for="job">Работа:</label>
          <input type="text" id="job" v-model="job" required>
        </div>
  
        <button type="submit" class="create-button">Создать</button>
      </form>
  
      <div v-if="createdUser" class="created-user">
        <h3 class="created-user-title">Пользователь создан</h3>
        <p><strong>ФИО</strong> {{ createdUser.name }}</p>
        <p><strong>Работа</strong> {{ createdUser.job }}</p>
        <p><strong>ID:</strong> {{ createdUser.id }}</p>
        <p><strong>Дата создания</strong> {{ createdUser.createdAt }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        name: '',
        job: '',
        createdUser: null
      };
    },
    methods: {
      createUser() {
        axios
          .post('https://reqres.in/api/users', {
            name: this.name,
            job: this.job
          })
          .then(response => {
            this.createdUser = response.data;
          })
          .catch(error => {
            console.error(error);
          });
      }
    }
  };
  </script>
  
  <style>
  .create-user {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f0f0f0;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .create-user-title {
    margin-top: 0;
    margin-bottom: 20px;
    font-size: 24px;
    color: #333;
  }
  
  .create-user-form {
    margin-bottom: 20px;
  }
  
  .form-group {
    margin-bottom: 10px;
  }
  
  label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
    color: #333;
  }
  
  input[type="text"] {
    width: 100%;
    padding: 8px;
    font-size: 16px;
    border-radius: 4px;
    border: 1px solid #ccc;
  }
  
  .create-button {
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 8px 16px;
    font-size: 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .create-button:hover {
    background-color: #0056b3;
  }
  
  .created-user-title {
    margin-top: 0;
    margin-bottom: 10px;
    font-size: 18px;
    color: #333;
  }
  
  .created-user p {
    margin-bottom: 5px;
  }
  </style>
  