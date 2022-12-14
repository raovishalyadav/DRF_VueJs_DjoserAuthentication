<template>
    <!-- <div class="login text-center"> -->

        <div class="login shadow-lg rounded-3 col-3 p-5 text-center bg-light text-dark centerlogin">
            <form @submit.prevent="submitForm">
                    <font-awesome-icon icon="fa-solid fa-circle-user" size="6x" style="color:purple" />
                <p class="mb-4 fw-bold fs-2" style="font-family: Verdana,monospace ">USER LOGIN</p>
                <div class="input text-start">
                    <font-awesome-icon icon="fa-solid fa-envelope" /> <label for="email" class="fs-6">Email
                        Address</label>
                    <input class="form-control bg-info bg-opacity-10 border-0 border-bottom border-info" type="email"
                        name="username" v-model="username" placeholder=" Type your email" />
                </div>
                <div class="input text-start mt-1">
                    <font-awesome-icon icon="fa-solid fa-key" /> <label for="password" class="fs-6">Password</label>
                    <input class="form-control bg-info bg-opacity-10 border-0 border-bottom border-info" type="password"
                        name="password" v-model="password" placeholder=" Type your password" />
                </div>

                <button type="submit" class="mt-4 btn btn-dark rounded-5 col-12 text-light fw-bold fs-5"
                    style="background: linear-gradient(to right,blue,purple)">
                    Sign in
                    <font-awesome-icon icon="fa-solid fa-right-to-bracket" />
                </button>

                <router-link to="/sign-up">
                <button type="submit" class="mt-4 btn btn-dark rounded-5 col-12 text-light fw-bold fs-5"
                style="background: linear-gradient(to right,green,black)">
                Sign up ?
                <font-awesome-icon icon="fa-solid fa-right-to-bracket" />
            </button></router-link>

            </form>
        </div>



        <!-- 
    <form @submit.prevent="submitForm">
        <input type="email" name="username" v-model="username">
        <input type="password" name="password" v-model="password">
        <button type="submit" class="btn btn-primary">Log me IN!</button>
    </form> -->
    <!-- </div> -->


</template>
    
<script>
import axios from 'axios';

export default {
    name: 'login',

    data() {
        return {
            username: '',
            password: ''
        }
    },

    methods: {
        submitForm(e) {
            const formData = {
                username: this.username,
                password: this.password
            }

            axios
                .post('/security/token/login/', formData)
                .then(response => {
                    console.log(response)
                    const token = response.data.auth_token
                    console.log(token, 'no token found:(((')
                    this.$store.commit('setToken', token)
                    axios.defaults.headers.common['Authorization'] = 'Token ' + token
                    localStorage.setItem('token', token)

                    const user = this.$store.state.isAuthenticated
                    if (user)
                    {
                    this.$router.push('/apidata')
                    }

                })
                .catch(error => {
                    console.log(error)
                })
        }
    }
}

</script>

<style scoped>
.centerlogin {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
</style>