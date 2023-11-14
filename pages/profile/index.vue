<template>
  <main class="profilePage">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>PROFILE</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div class="profile-pic">
            <img
              v-if="!imageData"
              src="../../assets/img/avatar.jpg"
              alt="null nullnull nulll"
            />
            <img
              v-if="imageData"
              :src="`${API_URL}/storage/users/${imageData}`"
              alt="no null no null"
            />
            <input
              ref="file"
              class="custom-file-input"
              type="file"
              @change="updateImage"
            />
          </div>
          <div class="profile-container">
            <div class="profile-info">
              <h2>{{ userData.name }}</h2>
              <span>{{ userData.username }}</span>
              <h1>{{ userData.role }}</h1>
            </div>
            <div class="button-edit">
              <NuxtLink to="/profile/editProfile"
                ><button class="btn">Edit Profile</button></NuxtLink
              >
              <NuxtLink to="/profile/editPassword"
                ><button class="btn">Edit Password</button></NuxtLink
              >
            </div>
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
  name: 'ProfilePage',
  data() {
    return {
      imageData: [],
      userData: [],
      API_URL: process.env.API_URL,
    }
  },

  mounted() {
    Promise.all([
      this.$axios.get(
        `api/image/${JSON.parse(localStorage.getItem('loginData')).id}`
      ),
      this.$axios.get(
        `api/auth/user/${JSON.parse(localStorage.getItem('loginData')).id}`
      ),
    ]).then((res) => {
      this.imageData = res[0].data
      this.userData = res[1].data.data
    })
  },

  methods: {
    async updateImage() {
      this.file = this.$refs.file.files[0]
      const formData = new FormData()
      formData.append('image', this.file)
      formData.append('_method', 'put')

      const config = {
        headers: {
          'Content-Type': 'multipart/form-data',
        },
      }

      try {
        await this.$axios
          .post(
            `api/updateimage/${this.userData.id}`,
            formData,
            config
          )
          .then((res) => {
            Swal.fire('Terima Kasih!', 'Image Sudah Di Update', 'success')
            this.$router.push({ path: '/profile' })
          })
      } catch (error) {
        Swal.fire('Oops!', 'Image lebih dari 2MB', 'error')
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.profilePage {
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
        flex-direction: row;
        align-items: center;
        margin-top: 3rem;
        margin-bottom: 2rem;
        // padding-bottom: 20px;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .profile-pic {
          display: flex;
          flex-direction: column;
          justify-content: center;
          margin-left: 250px;
          margin-bottom: 50px;
          margin-top: 50px;
          //   background-color: red;
          //   align-items: center;
          img {
            align-items: center;
            border-radius: 50%;
            border: solid 3px $primary;
            border-collapse: separate;
            border-spacing: 30px;
            width: 200px;
            height: 200px;
            margin-top: 20px;
            margin-bottom: 20px;
            padding: 5px 5px 5px 5px;
          }

          .custom-file-input {
            color: transparent;
          }
          .custom-file-input::-webkit-file-upload-button {
            visibility: hidden;
          }
          .custom-file-input::before {
            width: 230px;
            content: 'Ganti Foto';
            color: white;
            display: inline-block;
            background-color: $secondary;
            border: 1px solid #999;
            border-radius: 20px;
            padding: 5px 8px;
            outline: none;
            white-space: nowrap;
            user-select: none;
            cursor: pointer;
            font-weight: 700;
            font-size: 10pt;
            text-align: center;
          }
          .custom-file-input:hover::before {
            border-color: black;
          }
          .custom-file-input:active {
            outline: 0;
          }
          .custom-file-input:active::before {
            background: -webkit-linear-gradient(top, #e3e3e3, #f9f9f9);
          }
        }

        .profile-container {
          display: flex;
          flex-direction: column;
          align-items: center;
          height: auto;
          width: 90%;
          border-radius: 20px;
          background-color: #899799;
          margin-right: 250px;
          margin-bottom: 50px;
          margin-top: 50px;
          color: white;
          padding: 20px 20px 20px 20px;

          .profile-info {
            display: flex;
            flex-direction: column;
            align-items: center;
            border-bottom: solid 2px rgba($color: white, $alpha: 0.5);
            margin-bottom: 20px;

            h1 {
              text-transform: uppercase;
            }
          }
          .button-edit {
            display: flex;
            flex-direction: row;
            justify-content: space-evenly;
            // padding: 0 50px 0 50px;
          }
        }
      }
    }
  }
}
</style>
