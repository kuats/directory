<template>
  <div id="app">
    <header>
      <h1 class="app-title">Справочник Каптех</h1>
      <button v-if="isLoggedIn" class="logout-button" @click="logout">Logout</button>
    </header>

    <div class="main-content">
      <template v-if="isLoggedIn">
        <form class="user-list-form">
          <h2>Список пользователей</h2>
          <UserList />
        </form>
        <form class="single-user-form">
          <h2>Одиночный пользователь</h2>
          <SingleUser />
        </form>
        <form class="user-post-form">
          <h2>Создать пользователя</h2>
          <UserPost />
        </form>
        <form class="user-patch-form">
          <h2>Обновить пользователя</h2>
          <UserPatch />
        </form>
      </template>
      <template v-else>
        <UserLogin @loginSuccess="handleLoginSuccess" />
      </template>
    </div>
  </div>
</template>

<script>
import UserList from './components/UserList.vue';
import SingleUser from './components/SingleUser.vue';
import UserPost from './components/UserPost.vue';
import UserLogin from './components/UserLogin.vue';
import UserPatch from './components/UpdateUser.vue';

export default {
  name: 'App',
  components: {
    UserList,
    SingleUser,
    UserPost,
    UserLogin,
    UserPatch
  },
  data() {
    return {
      isLoggedIn: false,
      idleTimeout: null
    };
  },
  created() {
    window.addEventListener('mousemove', this.resetIdleTimer);
    window.addEventListener('keydown', this.resetIdleTimer);
    this.startIdleTimer();

    // Проверяем наличие сохраненного состояния авторизации при загрузке страницы
    const isLoggedIn = localStorage.getItem('isLoggedIn');
    if (isLoggedIn) {
      this.isLoggedIn = true;
    }
  },
  methods: {
    handleLoginSuccess() {
      this.isLoggedIn = true;
      this.startIdleTimer();
      // Сохраняем состояние авторизации при успешном входе
      localStorage.setItem('isLoggedIn', 'true');
    },
    logout() {
      this.isLoggedIn = false;
      clearTimeout(this.idleTimeout);
      // Удаляем сохраненное состояние авторизации при выходе
      localStorage.removeItem('isLoggedIn');
    },
    startIdleTimer() {
      clearTimeout(this.idleTimeout);
      this.idleTimeout = setTimeout(() => {
        this.logout();
      }, 10000); // Сбросить пользователя через 30 секунд бездействия
    },
    resetIdleTimer() {
      clearTimeout(this.idleTimeout);
      this.startIdleTimer();
    }
  }
};
</script>

<style>
#app {
  text-align: center;
  font-family: Arial, sans-serif;
  color: #333;
}

header {
  background-color: #f0f0f0;
  padding: 20px;
  margin-bottom: 20px;
}

.app-title {
  margin: 0;
}

.logout-button {
  background-color: #dc3545;
  color: #fff;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  border-radius: 4px;
  cursor: pointer;
}

.logout-button:hover {
  background-color: #c82333;
}

.main-content {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

form {
  margin-bottom: 40px;
}

form h2 {
  margin-bottom: 10px;
}

form .user-list-form,
form .single-user-form,
form .user-post-form,
form .user-patch-form {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

form:last-child {
  margin-bottom: 0;
}
</style>
