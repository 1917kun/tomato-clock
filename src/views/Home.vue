<template>
  <div id="home">
    <radial-progress-bar :diameter="500" :completed-steps="completedSteps" :total-steps="totalSteps" :inner-stroke-color="#323232">
   <p>Total : {{ timeleft }}</p>
   <p>Completed : {{}}</p>
   <b-btn variant="primary" v-if="status != 1" @click="start">
      <font-awesome-icon :icon="['fas','play']"></font-awesome-icon>
    </b-btn>
    <b-btn variant="primary" v-if="status == 1" @click="pause">
      <font-awesome-icon :icon="['fas','pause']"></font-awesome-icon>
    </b-btn>
    <h2>{{timetext}}</h2>
    <h1>{{currentText}}</h1>
      <!-- 如果目前有東西或是todos裡面有東西 -->
    <b-btn variant="primary" v-if="current.length > 0 || todos.length > 0"  @click="finish(true)">
      <font-awesome-icon :icon="['fas','step-forward']"></font-awesome-icon>
    </b-btn>
  </radial-progress-bar>

  </div>
</template>

<script>
export default {
  data () {
    return {
      // 0 = 停止
      // 1 = 播放
      // 2 = 暫停
      status: 0,
      timer: 0,
      innerStrokeColor: '#323232'
    }
  },
  computed: {
    currentText () {
      return this.current.length > 0 ? this.current : this.todos.length > 0 ? '點擊開始' : '沒有事項'
    },
    timetext () {
      const m = Math.floor(this.timeleft / 60)
      const s = Math.floor(this.timeleft % 60)
      return `${m} : ${s}`
    },
    alarm () {
      return this.$store.getters.alarm
    },
    timeleft () {
      return this.$store.getters.timeleft
    },
    current () {
      return this.$store.getters.current
    },
    todos () {
      return this.$store.getters.todos
    }
  },
  methods: {
    start () {
      if (this.status === 2) {
      // 暫停後繼續(這裡不需要檢查todo list裡面有沒有東西)
        this.status = 1
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft <= 0) {
            this.finish(false)
          }
        }, 1000)
      } else {
      // 開始新倒數(這裡需要去檢查todo list)
        if (this.todos.length > 0) {
          this.$store.commit('start')
          this.status = 1
          this.timer = setInterval(() => {
            this.$store.commit('countdown')
            if (this.timeleft <= 0) {
              this.finish(false)
            }
          }, 1000)
        }
      }
    },
    finish (skip) {
      clearInterval(this.timer)
      this.status = 0
      this.$store.commit('finish')

      if (!skip) {
        const audio = new Audio()
        audio.src = './alarms/' + this.alarm
        audio.play()
      }
      if (this.todos.length > 0) {
        this.start()
      } else {
        alert('結束')
      }
    },
    pause () {
      clearInterval(this.timer)
      this.status = 2
    }
  }

}
</script>
