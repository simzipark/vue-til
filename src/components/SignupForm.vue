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
    <div>
      <label for="nickname">NAME : </label>
      <input id="nickname" type="text" v-model="nickname" />
    </div>
    <button :disabled="!isUsernameVaild || !password || nickname" type="submit">
      SIGN UP
    </button>
    <p>{{ logMessage }}</p>
  </form>
</template>

<script>
import { registerUser } from '@/api/index';
import { validateEmail } from '@/utils/validation';

export default {
  data() {
    return {
      // form value
      username: '',
      password: '',
      nickname: '',
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
      const userdata = {
        username: this.username,
        password: this.password,
        nickname: this.nickname,
      };
      const { data } = await registerUser(userdata);
      this.logMessage = `${data.username} 님, 가입이 완료되었습니다.`;
      this.initForm();
    },
    initForm() {
      this.username = '';
      this.password = '';
      this.nickname = '';
    },
  },
};
</script>

<style></style>
