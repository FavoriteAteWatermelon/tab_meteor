<template>
  <div class="hello">
    <div >
      <div class="tabs-con" >
        <div class="item" @click="changeTabs(index,$event)" v-for="(item,index) in tabs" :key="index">
          <div :ref="'full'+ index" class="itemFull" v-show="tabActiviveIndex === index"></div>
          <div  :style="{color: tabActiviveIndex === index ? '#fff' : '#666'}" class="txt">{{item.txt}}</div>
        </div>
        <div  class="star-con" v-for="(item, i) in stars" :key="i +'1'">
          <transition  v-on:before-enter="beforeDropEnter"
            v-on:enter="dropEnter"
            v-on:after-enter="afterDropEnter">
                <!-- <div ref="star"  v-show="item.show" class="star">
                  <div class="inner"></div>
                </div> -->
                <!-- <div ref="star" :style="{transitionDelay: i === 0 ? '0' :'0.' + i + 's'}" v-show="item.show" class="star">
                  <div class="inner" :class="'inner' + (i + 1)" :style="{transitionDelay: i === 0 ? '0' :'0.' + i + 's'}"></div>
                </div> -->
                <div ref="star" :style="{transitionDelay: i === 0 ? '0' :'0.' + i + 's'}" v-show="item.show" class="star">
                  <div class="inner" :class="'inner' + (i + 1)" :style="{transitionDelay: i === 0 ? '0' :'0.' + i + 's'}" ></div>
                </div>
            </transition>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
const STARS_LENGTH = 5
function createStars () {
  let stars = []
  for (let i = 0; i < STARS_LENGTH; i++) {
    stars.push({show: false})
  }
  // console.log(stars)
  return stars
}
/* eslint-disable */
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      tabs: [
        {txt: 1},
        {txt: 2},
        {txt: 3},
        {txt: 4}
      ],
      stars: createStars(),
      tabActiviveIndex: 0,
      rect: {}
    }
  },
  methods: {
    changeTabs (i, event) {
      this.H = window.document.body.offsetHeight
      this.W = window.document.body.offsetWidth
      this.rect.x = event.x
      this.rect.y = event.y
      this.tabActiviveIndex = i
      this.stars.forEach((item, index) => {
        Vue.set(this.stars, index, {show: true})
        // console.log(this.stars)
      })
      this.$nextTick(() => {
        // x坐标
        event.target.children[0].style.left = (this.rect.x - (i * this.W / 4) ) + 'px'
        event.target.children[0].style.top = (80 - (this.H - this.rect.y) ) + 'px'
      })
      // this.tabActiviveIndex === 1 ? this.tabActiviveIndex = 0 : this.tabActiviveIndex = 1
      // console.log(this.tabActiviveIndex)
    },
    beforeDropEnter (el) {
      this.$refs['full' + this.tabActiviveIndex][0].style.transform = 'scale(1,1)'
        // console.log((this.W * (2 * this.tabActiviveIndex) / 8 - 4))
    },
    dropEnter (el) {

      this._reflow = document.body.offsetHeight
      const inner = el.getElementsByClassName('inner')[0]
        // console.log( (80 - (this.H - this.rect.y) - 36))
      el.style.transform = 'translate3d(' + (this.W * (2 * this.tabActiviveIndex) / 8 - 4) + 'px, 0, 0)'
      inner.style.transform = 'translate3d(0, ' + (80 - (this.H - this.rect.y) - 36) + 'px, 0)'
      el.style.webkitTransform = 'translate3d(' + (this.W * (2 * this.tabActiviveIndex) / 8 - 4) + 'px, 0, 0)'
      inner.style.webkitTransform = 'translate3d(0,' + (80 - (this.H - this.rect.y) - 36) + 'px, 0)'
      // el.style.display = 'block'


 
    },
    afterDropEnter (el) {
      // el.style.transform = 'translate3d(0, 0, 0)'

        // el.style.display = 'none'
        // inner.style.display = 'none'
       const inner = el.getElementsByClassName('inner')[0]
       console.log(this.tabActiviveIndex)
      inner.style.transform = 'translate3d(' + (this.W * (2 * (this.tabActiviveIndex)) / 8) + 'px, 0, 0)'
      // inner.style.display = 'b'
      // console.log(this.stars)
      el.style.webkitTransform = 'translate3d(0, 0, 0)'
      this.stars.forEach((item, index) => {
        Vue.set(this.stars, index, {show: false})
      })
      
      this.$refs['full' + this.tabActiviveIndex][0].style.transform = 'scale(30,30)';
  
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  scoped>
.tabs-con{
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 80px;
  /* background: red; */
  display: flex;
  overflow: hidden;
}
.item {
  position: relative;
  text-align: center;
  line-height: 80px;
  flex: 1;
  transition: all 0.5s;
  overflow: hidden;
  
  /* transition-delay: 0.s; */
}
.txt {
  position: absolute;
  top:50%;
  left: 50%;
  margin-left: -10px;
  margin-top: -20px;
  width: 20px;
  height: 40px;
  line-height: 40px;
}
.fullAll{
  transition: all 0.6s;
  transition: 0.6s;
  transform: scale(4);
}
.itemFull {
  position: absolute;
  transition: all 0.4s;
  top:0;
  left: 0;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: green;
 }
/* .tabs-con div{
  position: relative;
  flex: 1;
  line-height: 80px;
  text-align: center;
} */

.star-con,.star {
  position: absolute;
  /* transition: all 0.5s; */
  top: 50%;
  left: 12.5%;
  margin-top: -4px;
  margin-left: -4px;
  right: 200px;
  height: 8px;
  width: 100px;
  z-index: 200;
  transition: all 0.5s linear
  /* transition: all 0.6s linear */
}
.inner1 {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: green;
  /* transition: all 0.6s linear */
  transition: all 0.5s cubic-bezier(.17,.67,.3,1.36)
}
.inner2 {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: green;
  /* transition: all 0.6s linear */
  transition: all 0.5s cubic-bezier(.17,.67,1,-0.85)
}
.inner3 {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: green;
  /* transition: all 0.6s linear */
  transition: all 0.5s cubic-bezier(.02,-0.35,1,-0.85)
}

.inner4 {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: green;
  /* transition: all 0.6s linear */
  transition: all 0.5s cubic-bezier(.04,1.54,1,-0.85)
}
.inner5 {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: green;
  /* transition: all 0.6s linear */
  transition: all 0.5s cubic-bezier(.34,-0.12,.3,1.36)
}
.inner6 {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: green;
  transition: all 0.5s linear
  /* transition: all 0.6s cubic-bezier(1,-0.53,.3,1.36) */
}

</style>
