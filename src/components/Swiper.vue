<template>
  <ul>
      <li v-for="(item,index) in message" :key="item" :style="{background: colors[index]}">{{item}}</li>
  </ul>
</template>

<script>
export default {
  name: 'Swiper',
  data () {
    return {
      tag: 0,
      message: [
        'Review',
        'Study',
        'Play'
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
  ul li:nth-child(1){
    transform: translate3d(0,0,0);
    z-index:3;
    background: #c0dfd9;
  }
  ul li:nth-child(2){
    transform: translate3d(20px,-40px,-60px);
    z-index:2;
    background: #e9ece9;
  }
  ul li:nth-child(3){
    transform: translate3d(40px,-80px,-120px);
    z-index:1;
    background: #b3c2bf;
  }
  img{
    widows: 100%;
    display: block;
  }
</style>
