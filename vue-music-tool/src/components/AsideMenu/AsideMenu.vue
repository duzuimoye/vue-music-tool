<template>
    <transition name="fadein">
        <div class="aside-menu">
            <i @click="showAsideMenu" class="back"></i>
            <div class="aside">
                <div class="info" :style="{background:skinColor}">
                  <img src="https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2172011902,2298211924&fm=27&gp=0.jpg" alt="" class="avatar">
                  <div class="name">莫邪</div>
                  <div class="level">Lv60</div>
                  <div @click="isSignIn=true" class="sign">
                      <i></i>
                      {{isSignIn ? '已签到':'签到'}}
                  </div>
                </div>
            </div>
        </div>

    </transition>
</template>

<script>
export default {
    name:'AsideMenu',
    data(){
        return{
            isSignIn:false
        }
    },
    computed:{
        skinColor(){
            return this.$store.state.skinColor;
        }
    },
    methods: {
        showAsideMenu(){
            this.$store.commit('showAsideMenu',false);
        }
    }
}
</script>

<style>

</style>
