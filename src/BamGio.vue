<template>
  <div>{{ bieudien(tong) }}</div>
  <div>
    <template v-if="!isRunning">
      <button @click="reset">Đặt lại</button>
      <button @click="start"> Bắt đầu</button>
    </template>
    <template v-else>
      <button @click="themvong" >Vòng</button>
      <button @click="stop"> Dừng lại</button>
    </template>
  </div>
  <ul v-if="thoigianBamgio.length !== 1 || thoigianBamgio[0] !== 0">
    <li v-for="(todo, index) in thoigianBamgio" v-bind:key="index">
      <p>{{ chuvong }} {{index}} </p>
      <p>{{ bieudien(todo) }}</p>
    </li>
  </ul>
</template>

<script>
export default {
  name: "BamGio",
  data() {
    return {
      isRunning: false,
      mocThoigian: Date.now(),
      thoigianBamgio: [0],
      timelap: null,
      chuvong: 'vòng',
      index: 1,

    }
  },
  computed: {
    tong() {
      return this.thoigianBamgio.reduce((moc, todo) => {
        return moc + todo
      }, 0)
    }
  },
  methods: {
    bieudien(x) {

      let ms = x.toString().slice(-3, -1) || '00'
      function s() {
        let seconds = Math.floor((x/1000) % 60 )
        if (seconds < 10) {
          seconds = seconds.toString()
          seconds = '0' + seconds
          return seconds
        }
        return seconds.toString()
      }
      function p() {
        let minutes = Math.floor((x/60000) % 60)
        if (minutes < 10) {
          minutes = minutes.toString()
          minutes = '0' + minutes
          return minutes
        }
        return minutes.toString()
      }
      function h() {
        let hours = Math.floor((x / (1000 * 60 *60)) % 60)
        if (hours < 10) {
          hours = hours.toString()
          hours = '0' + hours
          return hours
        }
        return hours.toString()
      }
      return h() + ':' + p() + ':' + s() + ',' + ms
    },
    // tangvong() {
    //   'vòng' + this.todos.length
    // },
    start() {
      this.isRunning = true
      this.mocThoigian = Date.now()
      this.timelap = setInterval( () => {
        let timetinh = Date.now() - this.mocThoigian
        this.thoigianBamgio[0] += timetinh
        this.mocThoigian = Date.now()
      })
    },
    stop() {
      this.isRunning = false
      clearInterval(this.timelap)
      this.timelap = null
    },
    reset() {
      this.isRunning = false
      this.stop()
      this.thoigianBamgio = [0]
    },
    themvong() {
      // this.tangvong().unshift(0)

      this.thoigianBamgio.unshift(0)
    },
  }
}
</script>

<style scoped>

</style>