<template>
  <main class="site-inputMateri">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>DASHBOARD</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="courcard-head cards-style">
          <div class="form-materi">
            <form method="POST" @submit.prevent="onSubmit()">
              <div class="forms">
                <label for="namaMateri">Materi</label>
                <input
                  id="namaMateri"
                  v-model="namaMateri"
                  class="input-field"
                  type="text"
                  name="namaMateri"
                /><br />
              </div>
              <div class="forms">
                <label for="description">Keterangan</label>
                <textarea
                  id="description"
                  v-model="keterangan"
                  name="description"
                  rows="7"
                  cols="65"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="linkMateri">Link Materi</label>
                <input
                  id="linkMater"
                  v-model="idVideo"
                  class="input-field"
                  type="text"
                  name="linkMateri"
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
      namaMateri: '',
      keterangan: '',
      idVideo: '',
      data: [],
      API_URL: process.env.API_URL,
    }
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
            this.$router.push({path: '/materi'})
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
        height: 400px;
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

          .input-field{
            width: 500px;
            height: 50px;
            border-radius: 20px;
          }

          label {
            width: 150px;
            clear: left;
            text-align: left;
            padding-right: 10px;
            font-weight: 500;
          }

          textarea,
          input {
            margin-bottom: 20px;
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
