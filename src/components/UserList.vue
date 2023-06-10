<template>
  <div>
    <div class="user-list">
      <div
        v-for="(user, index) in userList"
        :key="index"
        :class="['user', 'parallax-effect', `user-${index}`]"
      >
        <div class="user-square">
          <div class="user-info">
            <span class="user-name">{{ user.first_name }} {{ user.last_name }}</span>
          </div>
          <div class="user-info">
            <span class="user-email">{{ user.email }}</span>
          </div>
          <img :src="user.avatar" :alt="user.first_name" class="user-photo" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userList: []
    };
  },
  mounted() {
    this.fetchUserList();
  },
  methods: {
    fetchUserList() {
      axios
        .get('https://reqres.in/api/users?page=2')
        .then(response => {
          this.userList = response.data.data;
          // Обработка успешного ответа от API
          this.animateUsers(); // Запуск анимации после получения списка пользователей
        })
        .catch(error => {
          console.error(error);
          // Обработка ошибок при выполнении запроса
        });
    },
    animateUsers() {
      const users = document.querySelectorAll('.user');

      // Запуск анимации для каждого пользователя с задержкой
      users.forEach((user, index) => {
        user.style.animationDelay = `${index * 0.4}s`;
      });
    }
  }
};
</script>

<style>
.user-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.user {
  flex-basis: 30%;
  margin-bottom: 20px;
  text-align: center;
  opacity: 0; /* Начальная прозрачность пользователя */
}

.user-square {
  background-color: #f0f0f0;
  border-radius: 10px;
  padding: 20px;
}

.user-info {
  margin-bottom: 5px;
}

.user-name {
  font-weight: bold;
}

.user-email {
  font-size: 14px;
  margin-bottom: 5px;
}

.user-photo {
  width: 100%;
  height: auto;
  border-radius: 10px;
}

/* Анимация появления пользователей */
@keyframes userAppear {
  0% {
    opacity: 0;
    transform: translateX(100%);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

/* Параллакс эффект */
.parallax-effect {
  animation: userAppear 1s ease forwards; /* Замедление анимации до 1 секунды */
}
.user-0 {
  animation-delay: 0s;
}
.user-1 {
  animation-delay: 0.4s;
}
.user-2 {
  animation-delay: 0.8s;
}
.user-3 {
  animation-delay: 1.2s;
}
.user-4 {
  animation-delay: 1.6s;
}
.user-5 {
  animation-delay: 2s;
}
/* Добавьте дополнительные классы .user-N и animation-delay для большего количества пользователей */
</style>
