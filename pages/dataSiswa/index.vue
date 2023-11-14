<template>
  <main class="site-materi">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>DATA SISWA</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div class="button-input">
            <button class="button-link">
              <nuxt-link class="link-input" to="dataSiswa/inputSiswa"
                ><Icon class="icon-link" icon="ant-design:plus-outlined" />
                Tambah Siswa</nuxt-link
              >
            </button>
          </div>
          <div
            v-for="(data, index) in dataSiswa"
            :key="index"
            class="dropdown"
          >
            <div class="title">
              <div class="number">
                <span> {{ index + 1}} </span>
              </div>
              <div class="nama">
                <nuxt-link
                  class="link-video"
                  :to="{
                    path: '/dataSiswa/infoSiswa',
                    query: { idSiswa: data.id },
                  }"
                  ><span>{{ data.name }}</span></nuxt-link
                >
              </div>
              <div class="action-button">
                <button>
                  <nuxt-link
                    class="link-video"
                    :to="{
                      path: '/dataSiswa/editSiswa',
                      query: { idSiswa: data.id },
                    }"
                    ><Icon class="icon-edit" icon="ant-design:edit-outlined"
                  /></nuxt-link>
                </button>
                <button @click="deleteUser(data.id)">
                  <Icon class="icon-delete" icon="bx:trash" />
                </button>
              </div>
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
import { Icon } from '@iconify/vue2'
import PagesBackground from '~/components/PagesBackground.vue'
export default {
  name: 'MateriPage',
  components: {
    Icon,
    PagesBackground,
  },
  data() {
    return {
      dummy: 2,
      dataSiswa: [],
      API_URL: process.env.API_URL,
    }
  },

  mounted() {
    this.$axios.get(`api/auth/user?filter[role]=Murid`).then((res) => {
      this.dataSiswa = res.data.data
    })
  },

  methods: {
    async deleteUser(id) {
      await this.$axios
        .delete(`api/auth/user/${id}`)
        .then((res) => {
          Swal.fire('Terima Kasih!', 'Siswa Telah Dihapus', 'success')
          this.$router.push({path: '/dataSiswa'})
        })
        .catch((err) => {
          console.error(err)
        })
    },
  },
}
</script>

<style lang="scss" scoped>
.site-materi {
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
      min-height: 65vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      flex: 1;

      .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        // margin-top: 3rem;
        margin-bottom: 2rem;
        padding-bottom: 20px;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .button-input {
          margin-top: 20px;
          width: 90%;

          .button-link {
            background-color: $secondary;
            width: 200px;
            height: 30px;
            border-radius: 20px;
            border: none;

            .link-input {
              display: flex;
              flex-direction: row;
              color: white;
              text-decoration: none;

              .icon-link {
                margin-right: 10px;
                margin-left: 10px;
                font-size: 12pt;
              }
            }
          }
        }

        .dropdown {
          height: auto;
          width: 90%;
          border-radius: 20px;
          background-color: $secondary;
          margin-top: 20px;
          color: white;

          .title {
            padding: 10px 0px 10px 0px;
            display: flex;

            .number {
              width: 12.5%;
              border-right: solid 2px white;
              display: flex;
              align-items: center;
            }

            .nama {
              display: flex;
              align-items: center;
            }

            .action-button {
              display: flex;
              align-items: center;
              width: 100%;
              display: flex;
              justify-content: flex-end;
              button {
                font-size: 15pt;
                font-weight: bold;
                color: white;
                background: none;
                border: none;
                margin-right: 10px;
                cursor: pointer;
              }
            }

            .link-video {
              text-decoration: none;
              color: white;
            }

            span {
              padding: 0px 20px 0px 30px;
            }
          }
        }
      }
    }
  }
}
</style>
