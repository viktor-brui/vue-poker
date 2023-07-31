<template>
  <div class="auth-container">
    <h1>Sign in</h1>
    <form class="auth-form">
      <div class="auth-form_wrapper">
        <AuthInput v-model="user.login"/>
        <AuthInput v-model="user.password"/>
      </div>
      <button class="btn-auth" type="submit" @click.prevent="signIn">Sign in</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import router from '../../router';
import AuthInput from './AuthInput.vue'

const url = 'https://poker.evenbetpoker.com/api/web/v2/login?clientId=default'

const user = ref({
  clientId: 'default',
  login: 'richard',
  password: 'poker'
})

const signIn = async () => {
  const loginData = {
    method: 'POST',
    mode: 'cors',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(user.value)
  }

  fetch(url, loginData)
    .then((response) => {
      return response.json();
    })
    .then((response) => {
      const token = response.data[0].attributes.token
      const refreshToken = response.data[0].attributes['refresh-token']
      window.localStorage.setItem('token', token)
      window.localStorage.setItem('refreshToken', refreshToken)
      router.push('/games')
    })
    .catch(() => {
      alert('Вы не авторизрвались, проверьте правильность введеных данных!')
    })
}
</script>

<style lang="sass" scoped>
.auth-container
  display: flex
  width: 100%
  justify-content: center
  flex-direction: column
  align-items: center
  padding-top: 180px
  .auth-form_wrapper
    margin-bottom: 15px
    padding-bottom: 15px
  .btn-auth
    width: 100%
    cursor: pointer
    padding: 10px 18px
    border-radius: 10px
    font-size: 22px
    font-weight: 600
    color: #ffffff
    background-color: #0d6efd
    border-color: #0d6efd
</style>
