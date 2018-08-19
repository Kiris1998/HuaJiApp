<template>
  <ul>
      <li v-for="(item,index) in message"
          :key="item"
          :style="[tansform(index),tansformMain(index)]"
          @touchstart.stop.capture="touchStart"
          @touchmove.stop.capture="touchMove"
          @touchend.stop.capture="touchEnd"
          @mousedown.stop.capture="touchStart"
          @mousemove.stop.capture="touchMove"
          @mouseup.stop.capture="touchEnd"
          @webkit-transition-end="onTransitionEnd(index)"
          @transitionend="onTransitionEnd(index)">{{item}}</li>
  </ul>
</template>

<script>
export default {
  name: 'Swiper',
  data () {
    return {
      tag: 0,
      position: {
        start: {
          x: 0,
          y: 0
        },
        end: {
          x: 0,
          y: 0
        },
        currentPage: 0
      },
      behaviors: {
        poswidth: '',
        posheight: '',
        lastposwidth: '',
        lastposheight: '',
        tracking: false,
        opacity: 1,
        animate: false,
        swipe: false
      },
      message: [
        'Review',
        'Study',
        'Play',
        'Happy',
        'Delay',
        'Review3',
        'Study4',
        'Play2',
        'Happy4',
        'Delay5'
      ],
      colors: [
        '#c2dde6',
        '#bccdbe',
        '#e6e9f0',
        '#c0dfd9',
        '#e9ece9',
        '#b3c2bf'
      ]
    }
  },
  methods: {
    touchStart (e) {
      if (e.type === 'touchstart') {
        if (e.touches.length > 1) {
          this.behaviors.tracking = false
        } else {
          this.position.start.t = new Date().getTime
          this.position.start.x = e.targetTouches[0].clientX
          this.position.start.y = e.targetTouches[0].clientY
          this.position.end.x = e.targetTouches[0].clientX
          this.position.end.y = e.targetTouches[0].clientY
        }
      } else {
        this.position.start.t = new Date().getTime
        this.position.start.x = e.clientX
        this.position.start.y = e.clientY
        this.position.end.x = e.clientX
        this.position.end.y = e.clientY
      }
      this.behaviors.tracking = true
      this.behaviors.animate = false
    },
    touchMove (e) {
      if (this.behaviors.tracking && !this.behaviors.animate) {
        if (e.type === 'touchmove') {
          this.position.end.x = e.targetTouches[0].clientX
          this.position.end.y = e.targetTouches[0].clientY
        } else {
          this.position.end.x = e.clientX
          this.position.end.y = e.clientY
        }
        this.behaviors.poswidth = this.position.end.x - this.position.start.x
        this.behaviors.posheight = this.position.end.y - this.position.start.y
      }
    },
    touchEnd () {
      this.behaviors.tracking = false
      this.behaviors.animate = true
      if (Math.abs(this.behaviors.poswidth) > 100) {
        let ratio = Math.abs(this.behaviors.posheight / this.behaviors.poswidth)
        this.behaviors.poswidth = this.behaviors.poswidth > 0 ? this.behaviors.poswidth + 200 : this.behaviors.poswidth - 200
        this.behaviors.posheight = this.behaviors.posheight > 0 ? Math.abs(this.behaviors.poswidth * ratio) : -Math.abs(this.behaviors.poswidth * ratio)
        this.behaviors.opacity = 0
        this.behaviors.swipe = true
        this.behaviors.lastposwidth = this.behaviors.poswidth
        this.behaviors.lastposheight = this.behaviors.posheight
        this.position.currentPage++
        this.$nextTick(() => {
          this.behaviors.poswidth = 0
          this.behaviors.posheight = 0
          this.behaviors.opacity = 1
        })
      } else {
        this.behaviors.poswidth = 0
        this.behaviors.posheight = 0
        this.behaviors.swipe = false
      }
    },
    onTransitionEnd (index) {
      if (this.behaviors.swipe && index === this.position.currentPage - 1) {
        this.behaviors.animate = true
        this.behaviors.lastposwidth = 0
        this.behaviors.lastposheight = 0
        this.behaviors.swipe = false
      }
      console.log(index)
      console.log(this.position.currentPage)
      console.log(this.position.currentPage === this.message.length - 1)
    },
    tansformMain (index) {
      let style = {}
      if (index === this.position.currentPage && this.position.currentPage !== this.message.length - 1) {
        style['transform'] = 'translate3D(' + this.behaviors.poswidth + 'px,' + this.behaviors.posheight + 'px,0px)'
        style['opacity'] = this.behaviors.opacity
        style['z-index'] = 10
        if (this.behaviors.animate) {
          style['transitionTimingFunction'] = 'ease'
          style['transitionDuration'] = 300 + 'ms'
        }
      }
      style['background'] = this.colors[index % 6]
      return style
    },
    tansform (index) {
      if (index > this.position.currentPage && this.position.currentPage !== this.message.length - 1) {
        let style = {}
        let pages = 3
        let myPageNumber = index - this.position.currentPage
        if (index > this.position.currentPage && myPageNumber < pages) {
          style['transform'] = 'translate3D(0,0,' + -1 * myPageNumber * 60 + 'px)'
          style['opacity'] = 1
          style['z-index'] = myPageNumber * -1 + pages
        } else {
          style['z-index'] = -1
        }
        style['background'] = this.colors[index % 6]
        return style
      } else if (index === this.position.currentPage - 1) {
        let style = {}
        return style
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  ul{
    perspective: 900px;
    perspective-origin: 50% 150%;
    margin: 0;
    padding: 0;
    position: relative;
    width: 100%;
    display: flex;
    height: 100%;
  }
  ul li{
    list-style: none;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    position: absolute;
    text-align: center;
    overflow: hidden;
    border-radius: 10px;
    justify-content: center;
    align-items: center;
    color: #f6f1ed;
    font-weight: bolder;
    font-size: 30px;
    font-family: 'Montserrat', sans-serif;
  }
</style>
