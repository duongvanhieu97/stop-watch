<template>
  <div id="timer">
    <p class="thoigianchay">
      <span class="hour">{{ gio }}</span>:
      <span class="minutes">{{ phut }}</span>:
      <span class="seconds">{{ giay }}</span>,
      <span class="milliseconds">{{ minigiay }}</span>
    </p>
  </div>
  <div>
    <template v-if="!isRunning">
      <button @click="reset()">Đặt lại</button>
      <button @click="start()"> Bắt đầu</button>
<!--      <button @click="reset">ssss</button>-->
    </template>
    <template v-else>
      <button @click="themvong()" >Vòng</button>
      <button @click="stop()"> Dừng lại</button>
    </template>
  </div>
  <div>
    <div v-for="todo in todos" v-bind:key="todo">
      <div>
        <p>{{todo.title}}</p>
        <p>{{ gio }}:{{ phut}}:{{giay}},{{minigiay}}</p>
      </div>
      <p>{{ todo.title }} {{ bieudien(todo.tinhtoan) }}</p>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      time: 0,
      mocBatDau: null,
      // minigiay: null,
      isRunning: false,
      interval: null,
      timereset: null,
      // dem: 0,
      clock: null,
      todos: []

    }
  },
  computed: {

    minigiay() {
      return this.time.toString().slice(-3,-1) || "00"
    },

    giay() {
      let giay = (this.time / 1000) % 60
      let giayString = giay.toString().indexOf('.')
      if (giay < 10) {
        if (giayString === -1) {
          giay = '00'
        }
        giay = giay.toString().slice(0,giayString)
        giay = '0' + giay
        return giay
      }else {
        return giay.toString().slice(0,giayString)
      }
    },

    phut() {
      let phut = (this.giay / 60) % 60
      let phutString = phut.toString().indexOf('.')
      if (phut < 10) {
        if (phutString === -1) {
          phut = '00'
        }
        phut = phut.toString().slice(0,phutString)
        phut = '0' + phut
        return phut
      }
      return phut.toString().slice(0,phutString)
    },
    gio() {
      let gio = (this.phut / 60) % 60
      let gioString = gio.toString().indexOf('.')
      if (gio < 10) {
        if (gioString === -1) {
          gio = '00'
        }
        gio = gio.toString().slice(0,gioString)
        gio = '0' + gio
        return gio
      }
      return gio.toString().slice(0,gioString)
    },
  },
  methods: {
    start() {
      this.isRunning = true
      this.mocBatDau = Date.now()
      this.interval = setInterval(() => {
        this.time = Date.now() - this.mocBatDau
      }, 1)
      this.mocBatDau = Date.now()
    },
    stop() {
      this.isRunning = false
      // this.time = null
      clearInterval(this.interval)
    },
    reset() {
      this.interval = null
      this.time = 0
        this.isRunning = false
    },
    // tangdem() {
    //   this.dem++;
    // },
    bieudien(x) {
      let ms = x.toString().slice(-3, -1)
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
    // datlai() {
    //   this.isRunning = true
    //   this.timereset = setInterval( () => {
    //     this.timechay = Date.now() - this.mocBatDau
    //   },1)
    // },

    themvong() {
      let array = this.todos
      let tongthoigian = array.reduce(function (totalTime, thoigian) {
        return totalTime + thoigian.tinhtoan;
      }, 0)
      this.todos.push({
        id: this.todos.length,
        title: 'vòng' + this.todos.length,
        tinhtoan: Date.now() - this.mocBatDau - tongthoigian

      })
    },
  },
}
</script>

<style scoped>

</style>