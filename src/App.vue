<template>
  <div id="app" v-cloak>
    <div class="components-container">
        <transition>
            <router-view></router-view>
        </transition>
        <md-button @click="goTop()" v-show="toTop" class="md-fab md-mini go-top">
          <md-icon>publish</md-icon>
        </md-button>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapMutations } from "vuex";
let timer = false;
export default {
  name: "app",
  data() {
    return {
      toTop: false
    };
  },
  mounted() {
    this.$nextTick(function() {
      window.addEventListener("scroll", this.needToTop); //滚动事件监听
    });
  },
  methods: {
    goTop() {
      // 回到顶部方法
      clearInterval(timer);
      timer = setInterval(function() {
        let curHeight =
          document.documentElement.scrollTop || document.body.scrollTop; // 得到当前高度
        var now = curHeight;
        var speed = (0 - now) / 7; // 随着高度减速
        speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);
        if (curHeight === 0) {
          //当前高度为零,停止这次计时器
          clearInterval(timer); // C1
        }
        document.documentElement.scrollTop = curHeight + speed; //直接给高度赋值,会调用needtotop方法
        document.body.scrollTop = curHeight + speed; //谷歌的
        stop = false; // A
      }, 30);
    },
    needToTop() {
      let curHeight =
        document.documentElement.scrollTop || document.body.scrollTop;
      let viewHeight = document.documentElement.clientHeight;
      if (curHeight > viewHeight + 100) {
        this.toTop = true; //赋值是为了按钮的v-show='toTop'
      } else {
        this.toTop = false;
      }
      if (stop) {
        //STOP
        clearInterval(timer); //C2
      }
      stop = true; //B
    }
  }
};
</script>

<style lang="scss">
v-cloak {
  display: none;
}
body {
  margin: 0;
  padding: 0;
}
p,
h1,
h2,
h3,
h4 {
  margin: 0;
}
#nprogress .bar {
  background: #3f51b5;
}
ul,
li {
  list-style: none;
}
ul {
  padding-left: 0px;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  height: 100%;
}
.components-container {
  height: 100%;
  -webkit-overflow-scrolling: touch;
}
.go-top {
  position: fixed !important;
  bottom: 20px;
  left: 0;
}
</style>
