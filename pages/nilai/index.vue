<template>
  <main class="nilaiPage">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>NILAI</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="info-content">
          <div class="info-container">
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
            </div>
            <div class="profile-info">
              <div class="head">
                <p>Nama</p>
                <p>Username</p>
              </div>
              <div class="body">
                <p>{{ userData.name }}</p>
                <p>{{ userData.username }}</p>
              </div>
            </div>
            <div class="score-info">
              <h2>Kriteria Kelulusan</h2>
              <table class="kriteria-kelulusan">
                <tbody>
                  <tr>
                    <td>Lulus</td>
                    <td><Icon icon="ic:baseline-greater-than-equal" /> 80</td>
                  </tr>
                  <tr>
                    <td>Tidak Lulus</td>
                    <td><Icon icon="ic:baseline-less-than" /> 80</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
          <h2>Nilai Ujian</h2>
          <div class="table">
            <table class="table-content">
              <thead>
                <tr>
                  <th>Nomor</th>
                  <th style="width: 40%">Subject / Materi</th>
                  <th style="width: 10%">Level 1</th>
                  <th style="width: 10%">Level 2</th>
                  <th style="width: 10%">Level 3</th>
                  <th style="width: 10%">Level 4</th>
                  <th style="width: 10%">Level 5</th>
                  <th style="width: 10%">Final Test</th>
                </tr>
              </thead>
              <tbody v-for="(data, index) in scores" :key="index">
                <tr>
                  <td style="text-align: center;">{{ index + 1 }}</td>
                  <td>{{ data.subject }}</td>
                  <td style="text-align: center;">{{ data.scores[1] }}</td>
                  <td style="text-align: center;">{{ data.scores[2] }}</td>
                  <td style="text-align: center;">{{ data.scores[3] }}</td>
                  <td style="text-align: center;">{{ data.scores[4] }}</td>
                  <td style="text-align: center;">{{ data.scores[5] }}</td>
                  <td style="text-align: center;">{{ data.scores[6] }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
import Swal from 'sweetalert2'
import { Icon } from '@iconify/vue2'
export default {
  name: 'ProfilePage',
  components: {
    Icon,
  },
  data() {
    return {
      imageData: [],
      userData: [],
      scores: null,
      subjectData: [],
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
      this.$axios.get(
        `api/exam/${JSON.parse(localStorage.getItem('loginData')).id}`
      ),
    ]).then((res) => {
      this.imageData = res[0].data
      this.userData = res[1].data.data
      this.scores = res[2].data.data
      console.log(this.scores)
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
.nilaiPage {
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

      .info-content {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 3rem;
        margin-bottom: 2rem;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .info-container {
          display: flex;
          flex-direction: row;
          align-items: center;
          height: auto;
          width: 90%;
          border-radius: 20px;
          background-color: #899799;
          margin-bottom: 10px;
          margin-top: 50px;
          color: white;
          padding: 20px 20px 20px 20px;
          .profile-pic {
            display: flex;
            flex-direction: column;
            justify-content: center;
            width: 40%;
            margin-left: 50px;
            img {
              align-items: center;
              border-radius: 50%;
              border: solid 3px white;
              border-collapse: separate;
              border-spacing: 30px;
              width: 200px;
              height: 200px;
              margin-top: 20px;
              margin-bottom: 20px;
              padding: 5px 5px 5px 5px;
            }
          }

          .profile-info {
            width: 80%;
            display: flex;
            flex-direction: row;
            align-items: flex-start;
            border-bottom: solid 2px rgba($color: white, $alpha: 0.5);
            margin-bottom: 20px;
            margin-left: 20px;
            margin-right: 200px;
            font-size: 14pt;
            // padding-right: 100px;
            h1 {
              text-transform: uppercase;
            }
          }

          .score-info {
            width: 80%;
            background-color: white;
            color: $secondary;
            padding: 20px;
            padding-left: 50px;
            margin-right: 50px;
            border-radius: 20px;
            .kriteria-kelulusan {
              font-size: 14pt;
            }
          }
        }
        .table {
          width: 90%;
          display: flex;
          justify-content: center;
          align-items: center;
          border-radius: 20px;
          background-color: $secondary;
          margin-bottom: 20px;
          color: white;
          // overflow-x: scroll;
          //   padding: 20px 20px 20px 20px;
          .table-content {
            border: 2px solid white;
            font-size: 14pt;
            margin: 20px 0 20px 0;
            width: 1000px;
            // background-color: white;
            // overflow-x: hidden;
            th,
            td {
              border: 2px solid white;
              padding: 10px 10px 10px 10px;
            }
          }
        }
      }
    }
  }
}
</style>
