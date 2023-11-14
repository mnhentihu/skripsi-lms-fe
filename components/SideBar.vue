<template>
  <main>
    <div class="Sidebar">
      <div v-if="data.role == 'Admin'" class="SidebarSideBar-admin">
        <SidebarAdmin />
      </div>
      <div v-if="data.role == 'Guru'" class="SidebarSideBar-guru">
        <SideBarGuru />
      </div>
      <div v-if="data.role == 'Murid'" class="SidebarSideBar-murid">
        <SidebarMurid />
      </div>
    </div>
  </main>
</template>

<script>
// import SidebarGuru from 'SideBarGuru.vue'

// import SidebarAdmin from './SidebarAdmin.vue';

// import SidebarMurid from 'SidebarMurid.vue'
export default {
  name: 'SideBar',
  data() {
    return {
      data: [],
    }
  },
  mounted() {
    this.$axios.get(
        `api/auth/user/${JSON.parse(localStorage.getItem('loginData')).id}`
      ).then((res) => {
      this.data = res.data.data
    })
  },
}
</script>

<style></style>
