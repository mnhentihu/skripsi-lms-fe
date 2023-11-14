<template>
  <main class="profile-button" :class="{ 'is-expanded': isActive }">
    <div class="profile">
      <button class="btn-nostyle" @click.prevent="isActive = !isActive">
        <Icon class="icon-profile" icon="healthicons:ui-user-profile-outline" />
      </button>
      <div class="profile-info">
        <img
          class="imageStyle"
          :src="`${API_URL}/storage/users/${imageData}`"
          alt="profile-pict"
        />
        <div class="text">
          <div class="name">{{ userData.name }}</div>
          <div class="role">{{ userData.role }}</div>
          <NuxtLink to="/profile"
            ><button class="btn">Profile</button></NuxtLink
          >
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import { Icon } from '@iconify/vue2'
export default {
  name: 'ProfileButton',
  components: {
    Icon,
  },
  data() {
    return {
      imageData: [],
      userData: [],
      isActive: false,
      API_URL: process.env.API_URL,
      data: [],
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
}
</script>

<style lang="scss" scoped>
.profile-button {
  .profile {
    position: relative;
    top: 13px;
    right: 20px;
    display: flex;
    justify-content: flex-end;
    .btn-nostyle {
      .icon-profile {
        font-size: 50px;
        color: $secondary;
      }
    }

    .profile-info {
      z-index: 1;
      position: absolute;
      bottom: -18rem;
      display: none;
      width: 230px;
      height: 275px;
      border-radius: 30px;
      background-color: $primary;
      text-align: center;
      .imageStyle {
        border-radius: 50%;
        border: solid 3px white;
        border-collapse: separate;
        border-spacing: 30px;
        width: 100px;
        height: 100px;
        margin-top: 20px;
        padding: 5px 5px 5px 5px;
      }
      .text {
        margin-top: 20px;
        color: white;
        .name {
          font-weight: bold;
          margin-bottom: 10px;
        }
        .btn {
          margin-top: 20px;
          color: white;
          border: solid 1px white;
        }
      }
    }
  }
  &.is-expanded {
    .profile {
      .profile-info {
        display: block;
      }
    }
  }
}
</style>
