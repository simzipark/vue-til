<template>
  <form @submit.prevent="submitForm">
    <div>
      <label for="username">ID : </label>
      <input id="username" type="text" v-model="username" />
    </div>
    <div>
      <label for="password">PASS : </label>
      <input id="password" type="password" v-model="password" />
    </div>
    <button :disabled="!isUsernameVaild || !password" type="submit">
      LOGIN
    </button>
    <p>{{ logMessage }}</p>
  </form>
</template>

<script>
import { loginUser } from '@/api/index';
import { validateEmail } from '@/utils/validation';

export default {
  data() {
    return {
      // userdata
      username: '',
      password: '',
      // log
      logMessage: '',
    };
  },
  computed: {
    isUsernameVaild() {
      return validateEmail(this.username);
    },
  },
  methods: {
    async submitForm() {
      try {
        //비즈니스 로직
        const userdata = {
          username: this.username,
          password: this.password,
        };
        const {
          data: { user },
        } = await loginUser(userdata);
        this.logMessage = `${user.username} 님, 환영합니다.`;
      } catch (error) {
        // 에러 핸들링
        this.logMessage = error.response.data;
      } finally {
        this.initForm();
      }
    },
    initForm() {
      this.username = '';
      this.password = '';
    },
  },
};
</script>

<style></style>
