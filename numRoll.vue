<template>
  <div ref="changenum" class="changenum">
    <div v-for="(n, key) in length" :key="key" class="changenum__item">
      <div>0</div>
      <div>1</div>
      <div>2</div>
      <div>3</div>
      <div>4</div>
      <div>5</div>
      <div>6</div>
      <div>7</div>
      <div>8</div>
      <div>9</div>
    </div>
  </div>
</template>
<script>
export default {
  name: "changnum",
  props: {
    num: Number,
    start: Number
  },
  data () {
    return {
      length: 0,
      rollNum: [],
      timer: null
    }
  },
  methods: {
    init () {
      this.length = this.num.toString().length
      // 初始化值
      let start = []
      if (this.start) {
        let startStr = this.start.toString()
        let diff = this.length - startStr.length
        for (let i = 0; i < diff; i++) {
          start.push(0)
        }
        for (let i in startStr) {
          start.push(Number(startStr[i]))
        }
      }
      for (let i = 0; i < this.length; i++) {
        if (this.start) {
          this.rollNum[i] = - (start[i] * 14)
        } else {
          this.rollNum[i] = 0
        }
      }
    },
    roll () {
      let doms = this.$refs.changenum.childNodes
      let height = 14
      let entiretyHeight = -(height * 10)
      let count = this.start || 0
      this.timer = setInterval(() => {
        // 步进
        // 如果总数和起始值相等，不做步进处理
        if (this.num !== this.start) {
          this.rollNum[this.length - 1] -= height
        }
        // 进一
        for (let i = this.length - 1; i >= 0; i--) {
          if (this.rollNum[i] <= entiretyHeight) {
            this.rollNum[i] = 0
            if (typeof this.rollNum[i - 1] === 'number') {
              this.rollNum[i - 1] -= height
            }
          }
        }
        // 渲染动画
        doms.forEach((el, index) => {
          el.style.transform = `translateY(${this.rollNum[index]}px)`
        })
        // 达到限制关闭定时器
        count++
        if (this.num <= count) {
          clearInterval(this.timer)
          this.timer = null
        }
      }, 1000)
    }
  },
  created () {
    this.init()
  },
  mounted () {
    this.roll()
  },
  destroyed () {
    clearInterval(this.timer)
    this.timer = null
  }
}
</script>
<style lang="scss" scoped>
  .changenum {
    display: inline-flex;
    height: 14px;
    overflow: hidden;

    &__item {
      font-size: 14px;
      color: #fff;
      transition: .5s
    }
  }
</style>