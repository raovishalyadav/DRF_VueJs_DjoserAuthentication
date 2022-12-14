<template>
  <div class="container-fluid mt-5 pt-5 text-center">

    <div v-if="$store.state.isAuthenticated" class="logout text-center">
      <form @submit.prevent="logoutForm">
        <button type="submit" class="mt-3 btn btn-dark btn-lg rounded-5 col-2 text-light fw-bold fs-5"
          style="background: linear-gradient(to right,red,black)">Log out <font-awesome-icon icon="fa-solid fa-right-from-bracket" /></button>
      </form>
    </div>

  </div>

  <router-view />


</template>


<script>

import axios from 'axios'

export default {
  name: 'App',
  beforeCreate() {
    this.$store.commit('initializeStore')

    const token = this.$store.state.token
    console.log(token, 'no token found:(')

    if (token) {
      axios.defaults.headers.common['Authorization'] = 'Token ' + token
    }
    else {
      axios.defaults.headers.common['Authorization'] = ''
    }
  },
  methods: {
    logoutForm(e) {
      axios.post('/security/token/logout/')
        .then(response => {
          console.log(response)

          this.$store.commit('removeToken')
          axios.defaults.headers.common['Authorization'] = ''
          localStorage.setItem('token', '')

          const user = this.$store.state.isAuthenticated
          if (!user) {
            this.$router.push('/log-in')
          }

        })
        .catch(error => {
          console.log(error)
        })
    }
  },

  mounted() {
    const user = this.$store.state.isAuthenticated

    if (user) {
      this.$router.push('/apidata')
    }
    else {
      this.$router.push('/')
    }
  }
}

</script>


<style lang="scss">
body {

    padding-top: 70px;
    background: linear-gradient(to right,cyan,blue,black);

}

html {
  height: 100%;
}

.navbar {
  border-bottom: .4vh solid white;
  background: linear-gradient(to right, white, cyan, rgb(0, 230, 255), rgb(0, 180, 255), rgb(0, 160, 255), rgb(0, 130, 255), rgb(0, 100, 255));
}
</style>
