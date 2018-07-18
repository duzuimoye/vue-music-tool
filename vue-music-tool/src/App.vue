<template>
  <div id="app">
    <!-- 主界面 -->
    <transition name="show">
      <div v-show="isShowIndex" class="index">
        <!-- 侧边栏 -->
        <AsideMenu v-show="isShowAsideMenu"></AsideMenu>
        <!-- 头部 -->
        <Vheader></Vheader>
        <!-- tab页内容 -->
        <router-view></router-view>
        <!-- 尾部播放器 -->
        <Vfooter></Vfooter>
      </div>
    </transition>
    <transition name="showIndex">
      <play v-show="!isShowIndex"></play>
    </transition>
    <audio v-bind:src="audio.src || (musicData[0]&&musicData[0].src)" v-bind:autoplay="isPlaying" ref="audio" ></audio>
    <!-- 关于界面 -->
    <About v-if="isShowAbout"></About>
  </div>
</template>

<script>
import Vheader from '@/components/Header/Header.vue'
import Vfooter from '@/components/Footer/Footer.vue'
import AsideMenu from '@/components/AsideMenu/AsideMenu.vue'
import Play from '@/components/Play/Play.vue'
import About from '@/components/About/About.vue'
export default {
  name: 'App',
  components:{
    Vheader,
    Vfooter,
    AsideMenu,
    Play,
    About
  },
  beforeCreate(){
    this.$store.dispatch('getData');
  },
  mounted() {
    this.$store.commit('findDOM',{name: 'audio',dom:this.$refs.audio});
    this.$refs.audio.addEventListener('ended',() =>{this.next();});
    this.$refs.audio.addEventListener('error',() =>{
      this.next();
    });
  },
  computed: {
    isShowAsideMenu() {
      return this.$store.isShowAsideMenu;
    },
    isShowSearch(){
      return this.$store.state.isShowSearch;
    },
    isShowIndex() {
      return this.$store.state.isShowIndex;
    },
    isPlaying() {
      return this.$store.state.isPlaying
    },
    audio() {
      return this.$store.state.audio
    },
    DOM(){
      return this.$store.state.DOM;
    },
    isShowAbout() {
      return this.$store.state.isShowAbout
    },
    musicData() {
      return this.$store.state.musicData
    }
  }
}
</script>

<style lang="scss">
@import "./common/style/base.scss";

.showIndex-enter-active{
  transition: all 0.4s ease-out;
}
.showIndex-leave-active{
  transition: all 0 ease;
}
.showIndex-enter,.showIndex-leave-active{
  transform: translateY(350px);
  opacity: 0;
}
.show-enter-active {
  transition: all .4s ease;
}
.show-leave-active {
  transition: all 0 ease-out;
}
.show-enter, .show-leave-active {
  transform: translateX(-350px);
  opacity: 0;
}

.down-enter-active {
  transition: all .2s ease-in-out;
}
.down-leave-active {
  transition: all .4s ease-in-out;
}
.down-enter, .down-leave-active {
  transform: translateY(-250px);
  opacity: 0;
}
#app, .index{
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;

  div.search-page{
    position: absolute;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    z-index:1;

    .search-input{
      position: relative;
      flex: 1.1l;
      width: 100%;
      background-color: #fff;
      border: none;
      input{
        width: 90%;
        height: 100%;
        padding: 0 55px 0 30px;
        outline: none;
        border:none;
        font-size: 1.2rem;
      }
      i.icon-search{
        position: absolute;
        top: 9px;
        right: 55px;
        display: inline-block;
        width: 25px;
        height: 25px;
        background: url('./assets/search.svg') no-repeat;
        background-size: contain;
      }
      span{
        display: inline-block;
        position: absolute;
        right: 10px;
        top: 10px;
        width: 23px;  
        height: 23px;
        background:url('./assets/close.svg') no-repeat;
        background-size: contain;
      }
    }
    .mask{
      flex: 15;
    }
  }
}
</style>
