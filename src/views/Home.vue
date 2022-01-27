<template lang="pug">
#home
  b-container-fluid#container
    b-row.h-100.d-flex.m-0
      b-col.d-flex.p-0#leftBlock(cols='6')
        #toDo
          h1 {{ currentText }}
        #triangle
        #eventList
          b-table(:items="items" :fields="fields" show-empty striped hover)
            template(#cell(select)="data")
              b-btn.mx-1#selectBtn.mx-1(variant="outline-warning")
            template(#empty)
              p.text-center No Misson
            template(#cell(name)="data")
              span#listItem {{ data.value }}
            template(#cell(play)="data")
              b-btn.mx-1#playBtn(variant="outline-warning" v-if="status !== 1" @click="start")
                img(src='../assets/play.png' alt='play')
        #addEvent
          b-input-group(label="" label-for="newinput" invalid-feedback="字數太少")
            b-form-input#inputEvent(placeholder='Add A New Mission...'  v-model="newinput" :state="newinputstate" @keydown.enter="additem")
            b-input-group-prepend
            b-button#addBtn(@click="additem")
              img(src='../assets/plus.png' alt='plus')
        #notification
          #topNo
            p#text1 Recently completed--
            p#text2 More
          #btmNo
            img(src='../assets/checked.png' alt='checked')
            p The Completed Thing To Do Today
      b-col.p-0#rightBlock(cols='6')
        #tomato
          #tomatoTop1
          #tomatoTop2
          #tomato1
          #tomato2
          #tomatoBtm
            img(src='../assets/smile.png' alt='smile')
          #timeLeft
            h2 {{ timeText }}
          #timeBar
            b-progress(:value="timevalue" height="10px")
        b-btn#play.mx-1(variant="primary" v-if="status !== 1" @click="start")
          img(src='../assets/play.png' alt='play')
        b-btn#pause.mx-1(variant="primary" v-else @click="pause")
          img(src='../assets/pause.png' alt='pause')
        b-btn#skip.mx-1(variant="primary" v-if="current.length > 0" @click="finish (true)")
          img(src='../assets/cancel.png' alt='skip')
</template>

<script>
export default {
  data () {
    return {
      // 0 = 停止
      // 1 = 倒數中
      // 2 = 暫停
      status: 0,
      timer: 0,
      newinput: '',
      fields: [
        { key: 'select', label: '' },
        { key: 'name', label: '' },
        { key: 'play', label: '' }
      ],
      noActivated: false
    }
  },
  computed: {
    current () {
      return this.$store.state.current
    },
    items () {
      return this.$store.state.items
    },
    currentText () {
      return this.current.length > 0 ? this.current : this.items.length > 0 ? 'Click PLAY to Start' : 'No Mission'
    },
    timeleft () {
      return this.$store.state.timeleft
    },
    timeText () {
      const m = Math.floor(this.timeleft / 60).toString().padStart(2, '0')
      const s = Math.floor(this.timeleft % 60).toString().padStart(2, '0')
      return `${m} : ${s}`
    },
    newinputstate () {
      return this.newinput.length > 2 ? true : this.newinput.length === 0 ? null : false
    },
    timevalue () {
      const value = this.timeleft / 1500 * 100
      return value
    }
  },
  methods: {
    start () {
      if (this.status === 0 && this.items.length > 0) {
        this.$store.commit('start')
      }
      if (this.current.length) {
        this.status = 1
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft <= -1) {
            this.finish(false)
          }
        }, 1000)
      }
    },
    pause () {
      this.status = 2
      clearInterval(this.timer)
    },
    finish (skip) {
      clearInterval(this.timer)
      this.status = 0
      this.$store.commit('finish')

      if (!skip) {
        const audio = new Audio()
        audio.src = require('@/assets/' + this.$store.state.sound)
        audio.play()
      }

      if (this.items.length > 0) {
        this.start()
      }
    },
    additem () {
      if (this.newinput.length > 2) {
        this.$store.commit('additem', this.newinput)
        this.newinput = ''
      }
    },
    delitem (index) {
      this.$store.commit('delitem', index)
    },
    reactivated () {
      this.noActivated = false
    }
  }
}
</script>
