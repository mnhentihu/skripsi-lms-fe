<template>
  <main class="editPassword-Page">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>EDIT PASSWORD</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div class="forms-container">
            <form action="" @submit.prevent="updatePassword">
              <div class="forms">
                <label for="name">Password Baru</label>
                <input
                  v-model="password"
                  class="password-field"
                  type="text"
                  name="name"
                />
              </div>
              <div class="forms">
                <label for="confirm">Konfirmasi Password Baru</label>
                <input
                  v-model="password_confirmation"
                  class="password-field"
                  type="text"
                  name="uname"
                />
              </div>
              <div
                v-if="password !== password_confirmation"
                class="form-error-msg"
              >
                Password tidak cocok
              </div>
              <input class="submit" type="submit" value="Save" />
            </form>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
import Swal from 'sweetalert2'
export default {
  name: 'EditPassword',
  data() {
    return {
      userData: [],
      password: '',
      password_confirmation: '',
      API_URL: process.env.API_URL,
    }
  },

  mounted() {
    this.$axios
      .get(`api/auth/user/${JSON.parse(localStorage.getItem('loginData')).id}`)
      .then((res) => {
        this.userData = res.data.data
      })
  },

  methods: {
    async updatePassword() {
      const form = {
        password: this.password,
      }

      try {
        await this.$axios
          .put(
            `api/updatepassword/${this.userData.id}`,
            form
          )
          .then((res) => {
            Swal.fire('Terima Kasih!', 'Password Sudah Di Update', 'success')
            this.$router.push({ path: '/profile' })
          })
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.editPassword-Page {
  display: flex;
  .left {
    width: auto;
  }
  .right {
    display: flex;
    flex-direction: column;

    .header {
      display: flex;
      justify-content: space-between;
      border-bottom: solid 2px $secondary;
      box-shadow: 2px 2px 30px 2px #888;

      .head-text {
        margin-left: 20px;
        color: $secondary;
      }
    }
    .container {
      width: 100%;
      min-height: 90vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      flex: 1;

      .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 3rem;
        margin-bottom: 2rem;
        padding-bottom: 20px;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .forms-container {
          height: auto;
          width: 90%;
          border-radius: 20px;
          background-color: #899799;
          margin-top: 20px;
          color: white;
          padding: 20px 20px 20px 20px;

          .forms {
            display: flex;
            flex-direction: row;
            align-items: flex-start;
            width: auto;
            margin-bottom: 20px;

            label {
              width: 150px;
              clear: left;
              text-align: left;
              padding-right: 10px;
              font-weight: 500;
            }

            .password-field {
              width: 300px;
              height: 25px;
              border-radius: 20px;
              padding-left: 10px;
            }
          }
          .submit {
            width: 150px;
            border-radius: 20px;
            background-color: $primary;
            color: white;
            border: none;
            outline: 0;
          }
          .submit:hover {
            background-color: #27bbc0;
          }
        }
      }
    }
  }
}
</style>
