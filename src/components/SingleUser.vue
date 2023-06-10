<template>
    <div class="user-details">
      <div
        v-if="user"
        class="user-container"
        :class="{ 'enlarged': isEnlarged }"
        @click="toggleEnlarged"
      >
        <div class="user-name">
          <span class="user-name">{{ user.first_name }} {{ user.last_name }}</span>
        </div>
        <div class="user-email">
          <span class="user-email">{{ user.email }}</span>
        </div>
  
        <img :src="user.avatar" :alt="user.first_name" class="user-photo" />
      </div>
  
      <div v-else class="loading-message">
        <p>Loading user details...</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        user: null,
        isEnlarged: false
      };
    },
    mounted() {
      this.fetchUserDetails();
    },
    methods: {
      fetchUserDetails() {
        axios
          .get('https://reqres.in/api/users/2')
          .then(response => {
            this.user = response.data.data;
          })
          .catch(error => {
            console.error(error);
          });
      },
      toggleEnlarged() {
        this.isEnlarged = !this.isEnlarged;
      }
    }
  };
  </script>
  
  <style>
  .user-details {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f0f0f0;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .user-details-title {
    margin-top: 0;
    margin-bottom: 20px;
    font-size: 24px;
    color: #333;
  }
  
  .user-container {
    margin-bottom: 20px;
    transition: transform 0.5s ease;
    cursor: pointer;
  }
  
  .user-container.enlarged {
    transform: scale(2) rotateY(360deg);
    /* Добавьте любые другие стили для эффекта параллакса */
  }
  
  .user-info {
    margin-bottom: 10px;
  }
  
  .user-name {
    font-weight: bold;
    font-size: 18px;
    color: #333;
  }
  
  .user-email {
    font-size: 14px;
    margin-bottom: 5px;
    color: #555;
  }
  
  .user-photo {
    width: 200px;
    height: auto;
    border-radius: 4px;
  }
  
  .loading-message {
    text-align: center;
    margin-top: 20px;
    color: #777;
  }
  </style>
  