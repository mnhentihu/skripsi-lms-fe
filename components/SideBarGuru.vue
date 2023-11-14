<template>
  <div class="site-sidebar" :class="{ 'is-expanded': isActive }">
    <aside>
      <div class="button">
        <button class="btn-nostyle" @click.prevent="isActive = !isActive">
          <Icon class="icon-sidebar" icon="ep:menu" />
        </button>
        <span>MENU</span>
      </div>
      <div class="menu">
        <div class="home">
          <NuxtLink class="link" to="/home"
            ><button class="btn-nostyle">
              <Icon class="icon-sidebar" icon="bytesize:home" />
            </button>
            <span>HOME</span>
          </NuxtLink>
        </div>
        <div class="materi">
          <NuxtLink class="link" to="/materi"
            ><button class="btn-nostyle">
              <Icon class="icon-sidebar" icon="carbon:book" />
            </button>
            <span>MATERI</span></NuxtLink
          >
        </div>
        <div class="ujian">
          <NuxtLink class="link" to="/bankSoal"
            ><button class="btn-nostyle">
              <Icon class="icon-sidebar" icon="carbon:test-tool" />
            </button>
            <span>BANK SOAL</span></NuxtLink
          >
        </div>
        <div class="nilai">
          <NuxtLink class="link" to="/dataSiswa"
            ><button class="btn-nostyle">
              <Icon class="icon-sidebar" icon="iconoir:stats-report" />
            </button>
            <span>DATA SISWA</span></NuxtLink
          >
        </div>
      </div>
      <div class="logout">
        <NuxtLink class="link" to=""
          ><button class="btn-nostyle" @click="logout()">
            <Icon class="icon-sidebar" icon="charm:power" />
          </button>
          <span>LOGOUT</span>
        </NuxtLink>
      </div>
    </aside>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue2'
export default {
  name: 'SideBarGuru',
  components: {
    Icon,
  },
  data() {
    return {
      isActive: false,
    }
  },
  methods: {
    async logout() {
      await this.$auth.logout()
    },
  },
}
</script>

<style lang="scss" scoped>
.site-sidebar {
  position: relative;
  width: 70px;

  aside {
    position: fixed;
    height: 100vh;
    width: 70px;
    background-color: $primary;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    color: white;
    font-size: 20px;

    .link {
      display: flex;
      align-items: center;
      text-decoration: none;
      color: white;
      font-weight: bold;
    }

    span {
      display: none;
      margin-left: 30px;
      font-weight: bold;
    }
    .icon-sidebar {
      position: relative;
      left: 1rem;
      font-size: 40px;
      color: white;
    }
    .button {
      position: relative;
      top: 1rem;
      display: flex;
      align-items: center;
      margin-top: 5px;
    }
    .menu {
      position: relative;
      top: -7rem;
      height: 30%;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      .home,
      .materi,
      .ujian,
      .nilai {
        display: flex;
        align-items: center;
      }
    }
    .logout {
      position: relative;
      bottom: 1rem;
      display: flex;
      align-items: center;
    }
  }

  &.is-expanded {
    width: 230px;
    aside {
      width: 230px;
      span {
        display: block;
      }
    }
  }
}
</style>
