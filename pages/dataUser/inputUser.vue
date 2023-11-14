<template>
  <main class="site-inputMateri">
    <!-- <img src="~assets/img/bg.png" alt="" class="bgimage" /> -->
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>INPUT USER</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="courcard-head cards-style">
          <div class="form-materi">
            <form method="POST" @submit.prevent="onSubmit()">
              <div class="forms">
                <label for="namaMateri">Nama</label>
                <input
                  id="namaMateri"
                  v-model="name"
                  class="inputField"
                  type="text"
                  name="namaMateri"
                /><br />
              </div>
              <div class="forms">
                <label for="uname">Username</label>
                <input
                  id="userName"
                  v-model="username"
                  class="inputField"
                  type="text"
                  name="uname"
                /><br />
              </div>
              <div class="forms">
                <label for="role">Role</label>
                <select id="role" v-model="role" class="inputField" name="role">
                  <option selected="true" disabled="disabled" value="" hidden>
                    Pilih Role
                  </option>
                  <option value="Admin">Admin</option>
                  <option value="Guru">Guru</option>
                  <option value="Murid">Murid</option>
                  <br />
                </select>
              </div>
              <div class="forms">
                <label for="profile-pict">Foto Profile</label>
                <input
                  id="userName"
                  ref="file"
                  class="inputField"
                  type="file"
                  name="uname"
                /><br />
              </div>
              <button type="submit" class="btn">Save</button>
            </form>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import PageFooter from '~/components/PageFooter.vue'
// import SidebarSideBar from '~/components/SidebarSideBar.vue'
export default {
  name: 'InputMateriPage',
  components: { PageFooter },

  data() {
    return {
      // isSuccess: false,
      name: '',
      username: '',
      password: '',
      image: '',
      role: 'Murid',
      data: [],
      API_URL: process.env.API_URL,
    }
  },
  methods: {
    async onSubmit() {
      try {
        this.file = this.$refs.file.files[0]
        const formData = new FormData()
        formData.append('name', this.name)
        formData.append('username', this.username)
        formData.append('password', this.username + 123)
        formData.append('role', this.role)
        formData.append('image', this.file)
        // formData.append('_method', 'put')

        const config = {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        }
        await axios
          .post(`${this.API_URL}/api/auth/user`, formData, config)
          .then((res) => {
            Swal.fire('Terima Kasih!', 'Siswa Telah Ditambahkan', 'success')
            this.$router.push({ path: '/dataUser' })
          })
      } catch (error) {
        Swal.fire('Mohon Maaf!', 'Data yang dimasukkan salah', 'error')
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.site-inputMateri {
  display: flex;
  position: relative;
  .left {
    width: auto;
  }

  .right {
    display: flex;
    flex-direction: column;
    .header {
      display: flex;
      justify-content: space-between;
      border-bottom: solid 1px $secondary;
      box-shadow: 2px 2px 30px 2px #888;

      .head-text {
        margin-left: 20px;
        color: $secondary;
      }
    }
    .container {
      display: flex;
      justify-content: center;
      width: 100%;
      height: 75vh;

      .courcard-head {
        width: 95%;
        height: 260px;
        // margin-top: 50px;
        margin-bottom: 50px;
        background-color: $secondary;
        color: white;
        font-size: 14pt;

        .form-materi {
          display: flex;
          justify-content: left;
          align-items: flex-start;
          margin-left: 50px;
          margin-bottom: 20px;
          margin-top: 30px;
        }

        .forms {
          display: flex;
          flex-direction: row;
          align-items: flex-start;
          width: auto;

          label {
            width: 150px;
            clear: left;
            text-align: left;
            padding-right: 10px;
            font-weight: 500;
          }

          .inputField {
            margin-bottom: 20px;
            width: 500px;
            border-radius: 20px;
          }
        }
      }
    }
  }

  hr {
    border-width: 2px;
  }
}
</style>
