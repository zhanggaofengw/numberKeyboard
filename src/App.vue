<template>
  <div id="app">
    <keyboard-demo></keyboard-demo>
    <router-view/>
  </div>
</template>

<script type="text/ecmascript-6">
  import KeyboardDemo from './components/demo/demo.vue'
  export default {
    name: 'App',
    created() {
      this.$nextTick(() =>{
        this.setPx(document, window)
      })
    },
    methods: {
      setPx(doc, win) {
        let docEl = doc.documentElement
        let resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize'
        let recalc = function () {
          let clientWidth = docEl.clientWidth
          if (!clientWidth) return
          docEl.style.fontSize = 100 * (clientWidth / 375) + 'px'
        }
        if (!doc.addEventListener) return
        win.addEventListener(resizeEvt, recalc, false)
        doc.addEventListener('DOMContentLoaded', recalc, false)
      }
    },
    components: {
      KeyboardDemo
    }
  }
</script>

<style>
  html, body {
    height: 100%;
    min-height: 100%;
    width: 100%;
    line-height: 1;
    font-weight: 200;
    font-family: 'PingFang SC', 'STHeitiSc-Light', 'Helvetica-Light', Arial, sans-serif;
  }

  body {
    background: #fff;
    font-size: 14px;
    -webkit-text-size-adjust: none;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  }
</style>
