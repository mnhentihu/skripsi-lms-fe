<template>
  <footer>
    <div class="container">
      <div class="contact">
        <div class="about-us">
          <h3>ABOUT US</h3>
        </div>
        <div class="contact-info">
          <div class="no-hp">
            <Icon class="icon-whatsapp" icon="akar-icons:whatsapp-fill" />
            <span> 082245462700 </span>
          </div>
          <div class="email">
            <Icon class="icon-email" icon="codicon:mail" />
            <span>
              <a href="mailto: hentihu.natsir@gmail.com"
                >hentihu.natsir@gmail.com</a
              >
            </span>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
import { Icon } from '@iconify/vue2'
export default {
  name: 'PageFooter',
  components: {
    Icon,
  },
}
</script>

<style lang="scss" scoped>
.container {
  // position: absolute;
  // bottom: 0;
  // position: fixed;
  bottom: 0;
  left: 0;
  // width: 100%;
  width: 100%;
  background-color: #29c7cc;

  .contact {
    position: relative;
    left: 20px;
    width: 97%;
    color: white;

    .about-us{
      padding-top: 5px;
    }

    .contact-info {
      color: white;
      display: flex;
      flex-direction: column;

      .no-hp{
        display: flex;
        align-items: center;
        .icon-whatsapp{
          font-size: 30px;
          margin-right: 10px;
        }
      }
      .email {
        display: flex;
        align-items: center;
        margin-bottom: 20px;
        .icon-email{
          font-size: 30px;
          margin-right: 10px;
        }
        a:link {
          color: white;
          background-color: transparent;
          text-decoration: none;
        }

        a:visited {
          color: white;
          background-color: transparent;
          text-decoration: none;
        }

        a:hover {
          color: grey;
          background-color: transparent;
          text-decoration: none;
        }

        a:active {
          color: grey;
          background-color: transparent;
          text-decoration: none;
        }
      }
    }
  }
}
</style>
