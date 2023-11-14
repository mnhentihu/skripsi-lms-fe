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
          <h1>INPUT SISWA</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="courcard-head cards-style">
          <h3 class="margin-text">Edit Siswa</h3>
          <hr />
          <div class="form-materi">
            <form method="POST" @submit.prevent="onSubmit()">
              <div class="forms">
                <label for="namaMateri">Nama</label>
                <input
                  id="namaMateri"
                  v-model="dataSiswa.name"
                  class="inputField"
                  type="text"
                  name="namaMateri"
                /><br />
              </div>
              <div class="forms">
                <label for="uname">Username</label>
                <input
                  id="userName"
                  v-model="dataSiswa.username"
                  class="inputField"
                  type="text"
                  name="uname"
                /><br />
              </div>
              <button type="submit" class="btn">Submit</button>
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
      dataSiswa: [],
      API_URL: process.env.API_URL,
    }
  },


  mounted(){
    this.$axios(`api/auth/user/${this.$route.query.idSiswa}`).then(
      (res) => {
        this.dataSiswa = res.data.data
      }
    )
  },

  methods: {
    async onSubmit() {
      try {
        const form = {
          namaMateri: this.namaMateri,
          keterangan: this.keterangan,
          idVideo: this.idVideo,
        }
        await axios
        .post(`${this.API_URL}/api/materis`, form)
          .then((res) => {
            Swal.fire('Terima Kasih!', 'Materi Sudah Di Upload', 'success')
            this.$router.push({ path: '/materi' })
          })
      } catch (error) {
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
        height: 250px;
        // margin-top: 50px;
        margin-bottom: 50px;

        .form-materi {
          display: flex;
          justify-content: left;
          align-items: flex-start;
          margin-left: 50px;
          margin-bottom: 20px;
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
