<template>
  <main class="loginpage">
    <!-- <img src="~assets/img/bg.png" alt="" class="bgimage" /> -->
    <PagesBackground />
    <div class="container">
      <div class="card-div">
        <div class="head">
          <h2 class="head-login">Login</h2>
        </div>
        <form action="" @submit.prevent="login()">
          <div class="uname">
            <input
              id="uname"
              v-model="formLogin.username"
              class="inputs"
              type="text"
              name="uname"
              placeholder="Username"
              required
            /><br />
          </div>
          <div class="pass">
            <input
              id="pass"
              v-model="formLogin.password"
              class="inputs"
              type="password"
              name="pass"
              placeholder="Password"
              required
            />
          </div>
          <div class="button">
            <button class="btn">Login</button>
          </div>
        </form>
      </div>
    </div>
  </main>
</template>

<script>
import Swal from 'sweetalert2'
import PagesBackground from '~/components/PagesBackground.vue';
export default {
    name: "LoginPage",
    components: { PagesBackground },
    data(){
      return {
        formLogin: {
          username: '',
          password: '',
        },
        API_URL: process.env.API_URL,
      }
    },
    methods: {
    async login() {
      await this.$auth
        .loginWith('local', {
          data: this.formLogin
        })
        .then(res => {
          localStorage.setItem('loginData', JSON.stringify(res.data.data))
          this.$router.push({ path: '/home'})
        })
        .catch(err => {
          Swal.fire('Mohon Maaf!', 'Username atau Password anda Salah!', 'error')
          console.error(err)
        })
    }
  }
}
</script>

<style lang="scss" scoped>
.loginpage {
  position: relative;

  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;

    .card-div {
      margin-top: 200px;
      width: 400px;
      height: 400px;
      border: none;
      border-radius: 20px;
      box-shadow: 10px 10px 30px 15px #888;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background-color: white;
    }

    .head {
      color: black;
      margin-bottom: 3rem;
      text-transform: uppercase;

      .head-login {
        font-weight: 700;
        text-align: center;
      }
    }

    .button {
      display: flex;
      justify-content: center;
      width: 100%;
      margin-top: 20px;
    }

    .inputs {
      text-align: center;
      margin-bottom: 10px;
      height: 45px;
      width: 250px;
      border-radius: 20px;
      border: solid black 2px;
    }
  }
}
</style>
