<template>
  <div class="main-container">
    <UserBalance :user-balance="userBalance"/>
    <GameBar :game-list="gameList"/>
  </div>
</template>

<script setup>
import {onMounted, onDeactivated, ref} from 'vue';
import UserBalance from './UserBalance.vue';
import GameBar from './GameBar.vue';
import router from '../../router';

let isAuth = true

const baseApiUrl = import.meta.env.VITE_API_BASE_URL
const refreshTokenParamsUrl = '/auth/token?clientId=default'
const gameListParamsUrl = '/v2/casino/games?clientId=default'

const userBalance = ref(null)
const gameList = ref(null)

onMounted(() => {
  const getRefreshToken = () => {
    return window.localStorage.getItem('refreshToken')
  }

  const refreshToken = async () => {
    fetch(baseApiUrl + refreshTokenParamsUrl, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        clientId: 'default',
        refreshToken: window.localStorage.getItem('refreshToken')
      })
    })
      .then((response) => {
        return response.json();
      })
      .then(response => {
        const newRefreshToken = response['refresh-token']
        const newToken = response['token']
        window.localStorage.setItem('refreshToken', newRefreshToken)
        window.localStorage.setItem('token', newToken)
      })
      .catch(() => {
        window.localStorage.removeItem('refreshToken')
        window.localStorage.removeItem('token')
        isAuth = false
      }
    )
  }

  if (getRefreshToken()) {
    setInterval(() => {
      refreshToken()
      isAuth = true
    }, 1000 * 800)
  } else {
    isAuth = false
    alert('Вы не авторизованы!')
    router.push('/')
  }
})

onDeactivated(() => {
  window.localStorage.removeItem('token')
  window.localStorage.removeItem('refreshToken')
})

if (isAuth) {
  const getBalance = () => {
    const token = window.localStorage.getItem('token')
    const balanceParamsUrl = `/v2/users/me/balance?clientId=default&auth=${token}`
    fetch(baseApiUrl + balanceParamsUrl).then(
      response => response.json()
    ).then(response => {
      userBalance.value = response.data[0].attributes
    })
  }
  const getGameList = async () => {
    fetch(baseApiUrl + gameListParamsUrl).then(
      response => response.json()
    ).then(response => {
      gameList.value = response
    })
  }

  getBalance()
  setInterval(() => {
    getBalance()
  }, 1000 * 30)
  getGameList()
}
</script>

<style lang="sass">
.main-container
  height: 100%
  display: flex
  background: url('../../assets/main_image.jpg') no-repeat
  background-size: cover

@media screen and (min-width: 320px)
  .main-container
    flex-direction: column

@media screen and (min-width: 1440px)
  .main-container
    flex-direction: row
</style>
