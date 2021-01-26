<template>
  <div id="app">
    <div :class="(unveil ? 'swoop' : '' )" class="logo-loader">
      <svg id="logo" width="1647" height="1647" viewBox="0 0 1647 1647" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M419.821 404.413C423.557 184.292 603.161 7 824.175 7H1235.59H1640V36.3011C1640 239.604 1475.19 404.413 1271.89 404.413H1235.59H1228.59V411.413V822.826V859.127C1228.59 1062.43 1063.78 1227.24 860.476 1227.24H831.175V822.826V411.413V404.413H824.175H419.821Z" stroke="white" stroke-width="14"/>
        <path d="M817.174 1242.59V1271.89C817.174 1475.18 652.101 1640 448.448 1640H412.087H375.727C172.073 1640 7 1475.18 7 1271.89V1235.59V824.175V787.873C7 584.581 172.073 419.762 375.727 419.762H405.087V824.175V1235.59V1242.59H412.087H817.174Z" stroke="white" stroke-width="14"/>
      </svg>
    </div>
    <div :class="(unveil ? 'loading-veil unveil' : 'loading-veil')">
    </div>
    <div class="cc" :class="[ 'g-cursor', { 'g-cursor_hover': hover }, {'g-cursor_hide': hideCursor} ]">
      <div :style="cursorCircle" class="g-cursor__circle"></div>
      <div class="g-cursor__point" ref="point" :style="cursorPoint"></div>
    </div>
    <router-view :dataRef='dataRef' />
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    //CustomCursor
  },
  data() {
    return {
      dataRef: {},
      xChild: 0,
      yChild: 0,
      xParent: 0,
      yParent: 0,
      hover: false,
      hideCursor: false,
      unveil: null
    }
  },
  computed: {
    cursorCircle() {
      return `transform: translateX(${this.xParent}px) translateY(${this.yParent}px) translateZ(0) translate3d(0, 0, 0);`
    },
    cursorPoint() {
      return `transform: translateX(${this.xChild - 4}px) translateY(${this.yChild - 4}px) translateZ(0) translate3d(0, 0, 0);`
    }
  },
  mounted() {
    console.log('path length1: ', document.querySelector('#logo path').getTotalLength());
    setTimeout(() => {
      this.unveil = true;
      //this.waveOffset = 700;
      console.log('unveiled');
    }, 3000);
    document.addEventListener("mousemove", this.moveCursor);
    document.addEventListener('mouseleave', (e) => {
      this.hideCursor = true;
      console.log('left: ', e.target);
    });
    document.addEventListener('mouseenter', (e) => {
      this.hideCursor = false;
      console.log('entered: ', e.target);
    });
  },
  methods: {
    handleNav(index) {
      console.clear();
      console.log(index);
      console.log('ROUTE HOME');
      this.$refs.fullpage.api.moveTo(1);
    },
    moveCursor(e) {
      var self = this;
      //console.log(e.target.classList);
      if(e.target.classList.contains('hoverable') || e.target.classList.contains('fp-tooltip') || e.target.tagName == "SPAN") {
        console.log(e.target.tagName);
        //console.log('HOVERABLE:)');
        self.hover = true;
      } else {
        //console.log('NOT HOVERABLE!');
        self.hover = false;
      }
      this.xChild = e.clientX;
      this.yChild = e.clientY;
      setTimeout(() => {
        this.xParent = e.clientX - 20;
        this.yParent = e.clientY - 20;
      }, 100);
    }
  }
}
</script>

<style lang="scss">
html {
  transition: filter 1s;
}
.swoop {
  transform: translate(-22px, -22px) scale(0.3) !important;
  //background: green !important;
  transition: 1s cubic-bezier(0.65, 0, 0.35, 1);
}

#logo {
  width: 200px;
  height: 200px;
}

#logo path {
  stroke-dasharray: 4438.63525390625;
  stroke-dashoffset: 4438.63525390625;
  fill: transparent;
  animation: line-animate 3s cubic-bezier(0.65, 0, 0.35, 1) forwards, fill 1s ease forwards 3s;
}

@keyframes line-animate {
  100% {
    stroke-dashoffset: 0;
  }
}

@keyframes fill {
  to {
    fill: white;
  }
}

.logo-loader {
  position: absolute;
  top: 0px;
  left: 0px;
  transform: translate(calc(50vw - 100px), calc(50vh - 100px));
  margin: auto;
  z-index: 999999;
  width: 200px;
  height: 200px;
  transition: 2s cubic-bezier(0.65, 0, 0.35, 1);
}
.fp-slideNav {
  //margin-bottom: 400px !important;
  ul li a span {
    background: red !important;
  }
}
#fp-nav {
  top: 450px !important;
  margin-left: 40px;
  opacity: 0;
  transition: 1s;
  li {
    margin-bottom: 36px !important;
  }
  .fp-tooltip {
    font-size: 14px !important;
    padding-left: 32px;
    line-height: 22px;
  }
  .active span {
    opacity: 1 !important;
    background: white !important;
    border: 2px solid white;
    //transform: translate(4px,4px);
    &:hover {
      //transform: translate(-1px,-1px) !important;
      //transform: scale(1.5);
    }
  }
  span {
    border-radius: 4px !important;
    background: rgba(white, 0.2) !important;
    //background: rgba(0,0,0,0) !important;
    height: 16px !important;
    width: 16px !important;
    //transform: translate(-1px,-1px);
    transform: none !important;
    &:hover {
      //transform: translate(-1px,-1px);
      //transform: scale(1.05);
    }
  }
}
.unveil {
  opacity: 0 !important;
  transition: 4s;
  pointer-events: none;
}
.loading-veil {
  //display: none;
  background: black;
  opacity: 1;
  width: 100%;
  height: 100vh;
  position: fixed;
  margin: 0px;
  padding: 0px;
  pointer-events: none;
  z-index: 99999;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: 2s;
}
// ---------- transition shit -----------
//transition animation fade
.fade-enter, .fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
.fade-enter-active, .fade-leave-active {
  transition: all 300ms ease;
}
body {
  padding: 0px;
  margin: 0px;
  //cursor: none;
}
p {
  //font-family: 'Inconsolata', monospace !important; 
}
#app {
  font-family: 'Gotham', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  background: black; 
}
// --------- cursor stuff ----------

.g-cursor {
  position: absolute;
    &_hide {
      opacity: 0;
      width: 60px;
      height: 60px;
      transition: 
        width 1s ease,
        height 1s ease,
        opacity 1s ease;
    }
    &__circle {
      display: none;
      pointer-events: none;
      user-select: none;
      top: 2px;
      left: 2px;
      position: fixed;
      width: 36px;
      height: 36px;
      border: 2px solid rgba(white, 1);
      mix-blend-mode: difference;
      //background: rgba(white,0.3);
      border-radius: 100%;
      z-index: 5555;
      backface-visibility: hidden;
      transition: 
        margin 0.6s ease,
        opacity 0.6s ease,
        width 0.6s ease,
        height 0.6s ease,
    }
    &__point {
      top: 0;
      left: 0;
      position: fixed;
      width: 10px;
      height: 10px;
      pointer-events: none;
      mix-blend-mode: difference;
      user-select: none;
      border-radius: 100%;
      background: #fff;
      z-index: 55555555;
      backface-visibility: hidden;
      will-change: transform;
      transition:
        margin .4s ease,
        opacity .4s ease,
        width .4s ease,
        height .4s ease,
        transform .1s ease;
    }
    &_hover {
      .g-cursor__point {
          width: 64px;
          height: 64px;
          margin-left: -24px;
          margin-top: -24px;
          border: 2px solid rgba(white, 1);
          //background: white;
          //mix-blend-mode: difference;
          //background: rgba(white, 1);
          //border-color: white;
          transition: 
            margin .4s ease,
            width .4s ease,
            height .4s ease,
            opacity .4s ease,
            transform .1s;
        }
        .g-cursor__circle {
          opacity: 1;
          width: 64px;
          height: 64px;
          margin-left: -12px;
          margin-top: -12px;
          //background: rgba(white, 1);
          //border-color: transparent;
          transition: 
            margin .4s ease,
            width .4s ease,
            height .4s ease,
            opacity .4s ease;
        }
    }
}
@media only screen and (max-width: 900px) {
  .g-cursor {
    display: none !important;
  }
  .swoop {
      transform: translate(-56px, -62px) scale(0.2) !important;
      //background: green !important;
      transition: 1s cubic-bezier(0.65, 0, 0.35, 1);
   }
   .soc-container {
     transform: scale(0.7);
     top: 30px !important;
     right: 6px !important;
     display: none !important;
   }
   .landing-container {
     width: 300px !important;
     height: 80px !important;
   }
   .fp-tooltip {
     display: none !important;
   }
   #fp-nav {
     margin: 0px !important;
     transform: rotate(270deg);
     width: 12px;
     li {
       margin-bottom: 18px !important;
       a.active {
         width: 8px !important;
         height: 8px !important;
         margin: 0px 0 0 0px !important;
       }
     }
   }
}
</style>