<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
      <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
      <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
      <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view> <!-- 注意':seller="seller"' -->
    </keep-alive>
  </div>
</template>

<script>
import Header from '@/components/header/Header'

// const ERR_OK = 0

export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  created () {
    // this.$axios.get('/api/seller').then((response) => {
    //   if (response.data.errno === ERR_OK) {
    //     this.seller = response.data.data
    //     // console.log(this.seller)
    //   }
    //   // console.log(response)
    // })
    this.$axios.get('https://www.easy-mock.com/mock/59bc021ee0dc663341ac13a5/eleme/seller').then((response) => {
      console.log(response)
      this.seller = response.data.seller
    })
  },
  components: {
    'v-header': Header
  }
}
</script>

<style lang="scss" scoped>
#app {
  .tab {
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
      .tab-item {
        flex:1;
        text-align: center;
        & > a {
          display: block;
          text-decoration: none;
          font-size: 14px;
          color: rgb(77, 85, 93);
          &.router-link-active {
            color: rgb(240, 20, 20);
          }
        }
      }
  }
}
</style>
