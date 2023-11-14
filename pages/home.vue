<template>
  <main class="site-dashboard" :class="{ 'is-expanded': isActive }">
    <!-- <img src="~assets/img/bg.png" alt="" class="bgimage" /> -->
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
      <div class="container">
        <div class="slider">
          <figure>
            <div class="slide">
              <img src="~/assets/img/carousel1.png" />
            </div>

            <div class="slide">
              <img src="~/assets/img/carousel2.png" />
            </div>

            <div class="slide">
              <img src="~/assets/img/carousel3.png" />
            </div>

            <div class="slide">
              <img src="~/assets/img/carousel2.png" />
            </div>

            <div class="slide">
              <img src="~/assets/img/carousel1.png" />
            </div>
          </figure>
        </div>
        <div class="courcard-head cards-style">
          <h3 class="margin-text">Courses</h3>
          <hr />
          <div class="course-cards">
            <swiper class="swiper" :options="swiperOption">
              <swiper-slide
                v-for="dataMateri in dataMateri"
                :key="dataMateri.id"
                ><div class="courses cards-style">
                  <img
                    src="~assets/img/strukturatom.png"
                    alt=""
                    class="course-image"
                  />
                  <hr />
                  <h4 class="course-text">{{ dataMateri.namaMateri }}</h4>
                </div></swiper-slide
              >
              <div slot="pagination" class="swiper-pagination" ></div>
            </swiper>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
// import { Icon } from '@iconify/vue2'
// import SideBar from '../components/Sidebar/SideBar.vue'
import Profile from '~/components/Profile.vue'
import PagesBackground from '~/components/PagesBackground.vue'
export default {
  name: 'MateriPage',
  components: {
    // Icon,
    // SideBar,
    Profile,
    PagesBackground,
  },
  data() {
    return {
      isActive: false,
      dataMateri: [],
      API_URL: process.env.API_URL,
      swiperOption: {
        slidesPerView: 3,
        spaceBetween: 30,
        slidesPerGroup: 3,
        loop: true,
        loopFillGroupWithBlank: true,
        pagination: {
          el: '.swiper-pagination',
          clickable: true,
        },
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev',
        },
      },
    }
  },

  async fetch() {
    await this.$axios.get(`api/materis`).then((res) => {
      this.dataMateri = res.data.data
    })
  },
}
</script>

<style lang="scss" scoped>
.site-dashboard {
  display: flex;
  position: relative;
  .left {
    width: auto;
  }
  .right {
    display: flex;
    flex-direction: column;
    max-width: 94.3vw;
    // width: 90%;
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
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 100%;

      .slider {
        overflow: hidden;
        height: 500px;
        object-fit: cover;
        margin-bottom: 30px;
        box-shadow: 0px 5px 20px 5px #888888;
      }

      .slider figure div {
        width: 20%;
        float: left;
      }

      .slider figure img {
        width: 100%;
        float: left;
      }

      .slider figure {
        position: relative;
        width: 500%;
        margin: 0;
        left: 0;
        animation: 20s slidy infinite;
      }

      @keyframes slidy {
        0% {
          left: 0%;
        }

        10% {
          left: 0%;
        }

        12% {
          left: -100%;
        }

        22% {
          left: -100%;
        }

        24% {
          left: -200%;
        }

        34% {
          left: -200%;
        }

        36% {
          left: -300%;
        }

        46% {
          left: -300%;
        }

        48% {
          left: -400%;
        }

        58% {
          left: -400%;
        }

        60% {
          left: -300%;
        }

        70% {
          left: -300%;
        }

        72% {
          left: -200%;
        }

        82% {
          left: -200%;
        }

        84% {
          left: -100%;
        }

        94% {
          left: -100%;
        }

        96% {
          left: 0%;
        }

        100% {
          left: 0%;
        }
      }

      .cards {
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        height: 250px;
        width: 750px;
        margin-bottom: 50px;
      }

      .courcard-head {
        margin-bottom: 50px;
        width: 80%;
        padding-left: 20px;
        padding-right: 20px;

        .course-cards {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100%;
          height: 500px;
          padding-top: 50px;
          padding-bottom: 50px;
          .swiper {
            width: 100%;

            .swiper-slide {
              display: flex;
              justify-content: center;
              align-items: center;
              text-align: center;
              font-weight: bold;
              font-size: 14pt;
              background-color: $white;
            }
          }

          .courses {
            margin-left: 20px;
            margin-bottom: 50px;
            width: 300px;

            .course-image {
              border-radius: 5px;
              width: 100%;
            }

            .course-text {
              margin-left: 20px;
              margin-bottom: 20px;
            }
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
