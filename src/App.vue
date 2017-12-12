<template>
	<div id="app">
    <transition name="animation" mode="out-in">
		  <router-view class="transition"></router-view>
    </transition>
	</div>
</template>

<script>
export default {
  name: 'APP',
  data() {
    return {
      transitionName: ''
    }
  },

  watch: {
    $route(to, from) {
      if (to === '/dashboard' && from === '/login') {
        this.transitionName = 'animation'
      }
    }
  }
}
</script>

<style lang="scss">
@import "~normalize.css/normalize.css"; // normalize.css 样式格式化
@import "./styles/index.scss"; // 全局自定义的css样式

// .animation-enter-active{
//     position: absolute;
//     top: 0;
//     bottom: 0;
//     left: 0;
//     right: 0;
//     margin: auto;
//     animation: moveOut 1s ease both;
//   }
.transition {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
.animation-leave-active {
  animation: moveOut 1.5s ease both;
  background: #fff;
  .login-form {
    display: none;
  }

  &::after {
    content: "";
    display: block;
    height: 100%;
    width: 0%;
    position: absolute;
    top: 0;
    right: 0;
    background: #f2f2f2;
    animation: process 1.5s ease both;
  }
}

@keyframes moveOut {
  15% {
    width: 100%;
    height: 15px;
  }
  50% {
    width: 0%;
    height: 15px;
  }
  75% {
    opacity: 1;
    width: 100%;
    height: 15px;
  }
  100% {
    opacity: 0;
    width: 100%;
    height: 100vh;
  }
}

@keyframes process {
  49.9% {
    width: 0%;
  }
  50% {
    width: 100%;
  }
  100% {
    width: 100%;
  }
}

@import '~normalize.css/normalize.css'; // normalize.css 样式格式化
@import './styles/index.scss'; // 全局自定义样式
</style>
