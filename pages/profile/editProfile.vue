<template>
  <main class="editProfile-Page">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>EDIT PROFILE</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div class="forms-container">
            <form action="" @submit.prevent="updateProfile">
              <div class="forms">
                <label for="name">Nama</label>
                <input
                  v-model="name"
                  class="inputFields"
                  type="text"
                  name="name"
                />
              </div>
              <div class="forms">
                <label for="uname">Username</label>
                <input
                  v-model="username"
                  class="inputFields"
                  type="text"
                  name="uname"
                />
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
  name: 'EditProfile',
  data() {
    return {
      userId: '',
      name: '',
      username: '',
      API_URL: process.env.API_URL,
    }
  },

  mounted() {
    this.userId = JSON.parse(localStorage.getItem('loginData')).id
    this.name = JSON.parse(localStorage.getItem('loginData')).name
    this.username = JSON.parse(localStorage.getItem('loginData')).username
  },

  methods: {
    async updateProfile() {
      const form = {
        name: this.name,
        username: this.username,
      }
      await this.$axios
        .put(`${this.API_URL}/api/updateprofile/${this.userId}`, form)
        .then((res) => {
          Swal.fire('Terima Kasih!', 'Data Profile Sudah Di Update', 'success')
          this.$router.push({ path: '/profile' })
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>

<style lang="scss" scoped>
.editProfile-Page {
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

            .inputFields {
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
