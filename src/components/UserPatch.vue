<template>
    <div class="user-update">
  
      <div class="form-group">
        <label for="name">Имя:</label>
        <input id="name" v-model="name" type="text" />
      </div>
  
      <div class="form-group">
        <label for="job">Работа:</label>
        <input id="job" v-model="job" type="text"  />
      </div>
  
      <button class="update-button" @click.prevent="patchUser">Обновить вользователя</button>
  
      <div v-if="response" class="response">
        <p><strong>Имя:</strong> {{ response.name }}</p>
        <p><strong>Работа:</strong> {{ response.job }}</p>
        <p><strong>Обновлен в:</strong> {{ response.updatedAt }}</p>
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
        response: null
      };
    },
    methods: {
      patchUser() {
        const payload = {
          name: this.name,
          job: this.job
        };
  
        axios
          .patch('https://reqres.in/api/users/2', payload)
          .then(response => {
            this.response = response.data;
          })
          .catch(error => {
            console.error(error);
          });
      }
    }
  };
  </script>
  
  <style>
  .user-update {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f0f0f0;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .user-update-title {
    margin-top: 0;
    margin-bottom: 20px;
    font-size: 24px;
    color: #333;
  }
  
  .form-group {
    margin-bottom: 20px;
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
  
  .update-button {
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 8px 16px;
    font-size: 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .update-button:hover {
    background-color: #0056b3;
  }
  
  .response {
    margin-top: 20px;
    padding: 10px;
    background-color: #fff;
    border-radius: 4px;
    border: 1px solid #ccc;
  }
  
  .response-title {
    margin-top: 0;
    margin-bottom: 10px;
    font-size: 18px;
    color: #333;
  }
  
  .response p {
    margin-bottom: 5px;
  }
  </style>
  