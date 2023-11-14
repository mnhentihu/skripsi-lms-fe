<template>
  <main class="landingpage">
    <PagesBackground />
    <div class="container">
      <div class="head">
        <h2 class="welcome">WELCOME TO!</h2>
        <h1 class="kimia">
          <span
            ><svg
              xmlns="http://www.w3.org/2000/svg"
              class="quote-l"
              aria-hidden="true"
              role="img"
              width="60"
              height="60"
              preserveAspectRatio="xMidYMid meet"
              viewBox="0 0 24 24"
            >
              <path
                fill="black"
                fill-rule="evenodd"
                d="M7 21a4 4 0 0 1-4-4c0-1.473 1.333-6.14 4-14h2L7 13a4 4 0 1 1 0 8Zm10 0a4 4 0 0 1-4-4c0-1.473 1.333-6.14 4-14h2l-2 10a4 4 0 1 1 0 8Z"
              /></svg></span
          >Belajar Kimia Yuk!<span
            ><svg
              xmlns="http://www.w3.org/2000/svg"
              aria-hidden="true"
              role="img"
              width="60"
              height="60"
              preserveAspectRatio="xMidYMid meet"
              viewBox="0 0 24 24"
            >
              <path
                fill="black"
                fill-rule="evenodd"
                d="M17 11a4 4 0 1 1 4-4c0 1.473-1.333 6.14-4 14h-2l2-10ZM7 11a4 4 0 1 1 4-4c0 1.473-1.333 6.14-4 14H5l2-10Z"
              />
            </svg>
          </span>
        </h1>
        <h2>Mengenal Kimia! Ternyata Asyik dan Menarik!</h2>
      </div>
      <div class="get">
        <h3>Get Started!</h3>
      </div>
      <div class="login">
        <NuxtLink to="/login"><button class="btn">Login</button></NuxtLink>
      </div>
    </div>
  </main>
</template>

<script>
import PagesBackground from '~/components/PagesBackground.vue';
export default {
    name: "LandingPage",
    components: { PagesBackground }
}
</script>

<style lang="scss" scoped>
.landingpage {
  position: relative;
  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 700px;

    .head {
      text-align: center;

      .welcome {
        font-size: 50pt;
      }

      .kimia {
        font-size: 70pt;

        .quote-l {
          left: 90px;
        }
      }
    }

    .get {
      margin-top: 50px;
      text-align: center;
    }
    
    .login {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;

      .btn {
        width: 200px;
      }
    }
  }
}
</style>
