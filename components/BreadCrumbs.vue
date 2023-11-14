<template>
  <ul class="list-nostyle breadcrumbs">
    <nuxt-link to="/home"><Icon class="icon-home" icon="bytesize:home" /></nuxt-link>
    <li v-for="(item, index) in crumbs" :key="index" class="coba">
      <span class="icon-crumbs"> / </span>
      <span v-if="index === crumbs.length - 1">
        <nuxt-link :to="item.url">
          {{ item.title }}
        </nuxt-link>
      </span>
      <span v-else>
        <nuxt-link :to="item.url">{{ item.title }}</nuxt-link>
      </span>
    </li>
  </ul>
</template>

<script>
import { Icon } from '@iconify/vue2'
export default {
  name: 'BreadCrumbs',
  components: {
    Icon,
  },
  computed: {
    crumbs() {
      const pathsItem = []
      const fullPath = decodeURIComponent(this.$route.path)
      const items = fullPath.split('/')

      items.shift()

      // console.log(items.shift())

      items.forEach((b, i) => {
        b = b.split('-').join(' ')

        const url = items.slice(0, i + 1).toString()

        pathsItem.push({
          title: b,
          url: '/' + url.split(',').join('/'),
        })
      })
      // console.log(items)
      // console.log(pathsItem)
      return pathsItem
    },
  },
}
</script>

<style lang="scss" scoped>
.breadcrumbs {
  // background-color: red;
  display: flex;
  flex-wrap: wrap;
  font-size: 12pt;
  align-items: flex-start;
  text-transform: capitalize;
  // margin-bottom: 24px;
  margin: 20px;

  .icon-crumbs {
    display: inline-block;
    padding: 0 8px;
    color: #899799;
  }

  .icon-home{
    font-size: 12pt;
    font-weight: bold;
  }

  li a {
    color: $link;
  }

  li:last-child a {
    color: $black60;
  }
}

.coba {
  display: flex;
  flex-direction: rows;
}
</style>
