<template>
  <main class="videoPage">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>Video Materi</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div class="video">
            <iframe
              width="560"
              height="315"
              :src="idYoutube"
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
export default {
  name: 'videoMateri',
  data() {
    return {
      idYoutube: '',
      API_URL: process.env.API_URL,
    }
  },

  async fetch() {
    await this.$axios(`api/materis/${this.$route.query.id_Video}`).then(
      (res) => {
        this.idYoutube = res.data.data.idVideo
      }
    )
  },
}
</script>

<style lang="scss" scoped>
.videoPage {
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
      // min-height: 90vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      flex: 1;

      .content {
        display: flex !important;
        justify-content: center !important;
        align-items: center;
        // margin-top: 3rem;
        margin-bottom: 2rem;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .video {
          display: flex !important;
          justify-content: center !important;
          border-radius: 20px;
          background-color: $secondary;
          margin: 0 auto !important;

          .linkyoutube {
            margin-top: 50px;
            margin-bottom: 50px;
            width: 500px;
          }
        }
      }
    }
  }
}
</style>
