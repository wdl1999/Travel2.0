<template>
    <ul class="list">
        <li class="item" v-for="item of letters" :key="item"
        @click="handleClick" @touchstart.prevent="handleTouchStart" @touchmove="handleTouchMove"
        @touchend="handleTouchEnd">{{item}}</li>
    </ul>
</template>

<script>
export default {
  name: 'CityAlphabet',
  props: {
    cities: Object
  },
  data () {
    return {
      isTouch: false,
      startY: 0,
      timer: null
    }
  },
  computed: {
    letters () {
      const letters = []
      for (let item in this.cities) {
        letters.push(item)
      }
      return letters
    }
  },
  methods: {
    handleClick (e) {
      this.$emit('change', e.target.innerText)
    },
    handleTouchStart () {
      this.isTouch = true
    },
    handleTouchMove (e) {
      if (this.isTouch === true) {
        if (this.timer) {
          clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {
          const touchY = e.touches[0].clientY
          const index = Math.floor((touchY - this.startY) / 20)
          if (index >= 0 && index < this.letters.length) {
            this.$emit('change', this.letters[index])
          }
        }, 8)
      }
    },
    handleTouchEnd () {
      this.isTouch = false
    }
  },
  updata () {
    this.startY = this.$refs['A'][0].offsetTop
  }
}
</script>

<style lang="stylus" scoped>
@import '~styles/variables.styl'
.list
  position: absolute
  right: 0
  bottom: 0
  top: 1.58rem
  display: flex
  flex-direction: column
  justify-content: center
  width: .4rem
  .item
    height: .4rem
    color: $bgColor
    text-align: center
</style>
