<template>
  <div class="keyboardDemo">
    <ul class="number" data-num="focus" @click="numFocus($event)">
      <li :class="{active: isActive && (numList.length === 0 || numberFocusIndex === -1) }" @click="numberFocus(-1)">￥</li>
      <li v-for="num, index in numList" :key="index"
          :class="{active: isActive && numList.length > 0 && index === numberFocusIndex}"
          @click="numberFocus(index)">
        {{num}}
      </li>
    </ul>
    <transition name="fade">
      <calculation @confirmEvent="confirmEvent" :numberFocusIndex="numberFocusIndex"
                   @deleteEvent="deleteEvent" v-show="isActive" ref="calculation"></calculation>
    </transition>
    <div class="numDialog" @click="numNoFocus()" v-show="isActive">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Calculation from '../../base/calculation/calculation.vue'
  export default{
    data() {
      return {
        numList: [],
        isActive: false,
        numberFocusIndex: 0
      }
    },
    methods: {
      confirmEvent(list, type){
        this.numList = list
        if (this.numList.length > 1) {
          this.numberFocusIndex++
        }
        if (type === 'confirm') {
          this.numNoFocus()
        }
      },
      deleteEvent(list){
        this.numList = list
        if (this.numList.length > 0 && this.numberFocusIndex !== -1) {
          this.numberFocusIndex--
        }
      },
      numberFocus(index) {
        this.numberFocusIndex = index
      },
      /**
       * 获取焦点
       * @param e
       */
      numFocus(e) {
        if (e.target.nodeName === 'UL' && this.numList.length > 1) {
          this.numberFocusIndex = this.numList.length - 1
        }
        this.isActive = true
      },
      /**
       * 失去焦点
       */
      numNoFocus(){
        this.isActive = false
      }
    },
    components: {
      Calculation
    }
  }
</script>

<style>
  ul.number {
    position: absolute;
    z-index: 100;
    display: inline-block;
    height: 39px;
    width: 100%;
    padding: 0;
    line-height: 39px;
    background: #fff;
    border-bottom: 1px solid #ccc;
    border-radius: 0;
    font-size: .25rem;
    color: #c65a5a;
  }

  /*这里是闪烁光标*/
  ul.number li.active {
    animation: blink 1s infinite steps(1, end);
  }

  /*这里设置动画blink*/
  @keyframes blink {
    0%, 100% {
      border-right: 1px solid #66b1ff;
    }
    50% {
      border-right: 1px solid transparent;
    }
  }

  ul.number li {
    float: left;
    height: 37px;
    font-size: 23px;
    padding-right: 1px;
    box-sizing: border-box;
    border-right: 1px solid transparent;
  }

  li {
    list-style: none;
  }

  .numDialog {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }

  .fade-enter-active, .fade-leave-active {
    transition: all .15s
  }

  .fade-enter, .fade-leave-to {
    transform: translateY(2rem)
  }
</style>
