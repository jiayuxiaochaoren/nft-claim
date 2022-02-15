<template>
  <div id="app">
<!--   <div class="nav">-->
<!--     <router-link to="/">HOME</router-link>-->
<!--      <router-link to="/airdrop">AIRDROP</router-link>-->
<!--   </div>-->
    <router-view  v-if="isRouterAlive"/>
  </div>
</template>
<script>
export default{
  provide(){
    return {
      reload:this.reload
    }
  },
  data(){
    return {
      isRouterAlive:true
    }
  },
  methods:{
    reload(){
      this.isRouterAlive = false
      this.$nextTick(function (){
        this.isRouterAlive = true
      })
    }
  }
}
</script>
<style lang='less'>
  @import "styles/reset";

  .nav {
    text-align: center;

    a {
      color: #ccc;
      text-decoration: none;
      margin: 0 10px;
      padding: 2px 5px;
      display: inline-block;
      cursor: pointer;

      &.router-link-exact-active {
        color: #ffff;
        border-bottom: 2px solid #fff;
      }
    }
  }
</style>
