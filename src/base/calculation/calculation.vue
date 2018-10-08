<template>
  <div class='keyboard' @click.stop='_handleKeyPress'>
    <div class="clearFloat">
      <div class='key-row border'>
        <div class='key-cell' data-num='1'>1</div>
        <div class='key-cell' data-num='2'>2</div>
        <div class='key-cell' data-num='3'>3</div>
      </div>
      <div class='key-row item'>
        <div class='key-cell' data-num='4'>4</div>
        <div class='key-cell' data-num='5'>5</div>
        <div class='key-cell' data-num='6'>6</div>
      </div>
      <div class='key-delete' data-num='D'>
        <span data-num='D' class="deleteBg"></span>
      </div>
    </div>
    <div class="clearFloat">
      <div class='key-row border'>
        <div class='key-cell' data-num='7'>7</div>
        <div class='key-cell' data-num='8'>8</div>
        <div class='key-cell' data-num='9'>9</div>
      </div>
      <div class='key-row last'>
        <div class='key-cell empty' data-num='x'></div>
        <div class='key-cell' data-num='.'>.</div>
        <div class='key-cell' data-num='0'>0</div>
      </div>
      <div class='key-confirm' data-num='S'>确认</div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default{
    data() {
      return {
        num: []
      }
    },
    props: {
      numberFocusIndex: Number
    },
    methods: {
      //处理按键
      _handleKeyPress(e) {
        let num = e.target.dataset.num
        //不同按键处理逻辑
        switch (String(num)) {
          //小数点
          case '.':
            this._handleDecimalPoint()
            break
          //删除键
          case 'D':
            this._handleDeleteKey()
            break
          //确认键
          case 'S':
            this._handleConfirmKey()
            break
          case 'x':
            this._handleEmptyKey()
            break
          default:
            this._handleNumberKey(num)
            break
        }
      },
      _handleEmptyKey() {
        return false
      },
      //处理小数点函数
      _handleDecimalPoint() {
        //如果包含小数点，直接返回
        if (this.num.indexOf('.') !== -1) {
          return false
        }
        //如果小数点是第一位，补0
        if (this.num.length === 0 || this.numberFocusIndex === -1) {
          this.num.splice(this.numberFocusIndex + 1, 0, 0)
          this.$emit('confirmEvent', this.num)
          this.num.splice(this.numberFocusIndex + 2, 0, '.')
          this.$emit('confirmEvent', this.num)
          return false
        }
        if (this.num.length - this.numberFocusIndex > 2) {
          this.num.splice(this.numberFocusIndex + 3, this.num.length - this.numberFocusIndex - 3)
          this.num.splice(this.numberFocusIndex + 1, 0, '.')
        } else {
          this.num.splice(this.numberFocusIndex + 1, 0, '.')
        }
        this.$emit('confirmEvent', this.num)
      },
      //处理删除键
      _handleDeleteKey() {
        if (this.numberFocusIndex === -1){
          return
        }
        //如果没有输入，直接返回
        if (this.num.length === 0) {
          return false
        }
        console.log(this.numberFocusIndex)
        //否则删除
        this.num.splice(this.numberFocusIndex, 1)
        this.$emit('deleteEvent', this.num)
      },
      _handleConfirmKey() {
        this.$emit('confirmEvent', this.num, 'confirm')
      },
      //处理数字
      _handleNumberKey(num) {
        //如果有小数点且小数点位数不小于2
        if (this.num.indexOf('.') > -1) {
          if (this.num.length - (this.num.indexOf('.') + 1) < 2 || this.numberFocusIndex < this.num.indexOf('.')) {
            this.num.splice(this.numberFocusIndex + 1, 0, num)
            this.$emit('confirmEvent', this.num)
          }
        }
        //没有小数点
        if (!(this.num.indexOf('.') > -1)) {
          //如果第一位是0，只能输入小数点
          if (num == 0 && this.num.length == 0) {
            this.num.splice(this.numberFocusIndex + 1, 0, 0)
            this.$emit('confirmEvent', this.num)
            this.num.splice(this.numberFocusIndex + 2, 0, '.')
            this.$emit('confirmEvent', this.num)
          } else {
            this.num.splice(this.numberFocusIndex + 1, 0, num)
            this.$emit('confirmEvent', this.num)
          }
        }
      }
    }
  }
</script>

<style>
  .clearFloat:after {
    content: "";
    display: table;
    clear: both;
  }

  .keyboard {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 200;
    background: #fff;
    border-top: 1px solid #ccc;
  }

  div.keyboard > div {
    padding: 0;
  }

  div.keyboard > div > div {
    float: left;
  }

  .key-row {
    width: 75%;
  }

  .key-row.item {
    border-bottom: 1px solid #ccc;
  }

  .key-cell {
    float: left;
    width: 33.3%;
    text-align: center;
    height: .5rem;
    line-height: .5rem;
    font-size: 18px;
    border-right: 1px solid #ccc;
    box-sizing: border-box;
  }

  .key-row.border .key-cell {
    border-bottom: 1px solid #ccc;
  }

  .key-row.last .key-cell {
    border-bottom: 0;
  }

  .key-delete, .key-confirm {
    float: right;
    width: 25%;
    height: 1rem;
    line-height: 1rem;
    text-align: center;
    margin-top: -0.5rem;
  }

  .key-delete {
    display: flex;
    align-items: center;
    justify-content: center;
    border-bottom: 1px solid #ccc;
  }

  .key-delete:hover .deleteBg {
    /*background: url('./delete1.png') no-repeat;*/
  }

  .deleteBg {
    display: inline-block;
    width: 32px;
    height: 32px;
    background: url('./delete1.png') no-repeat;
  }

  .key-cell.empty {
    background: #ccc;
  }
</style>
