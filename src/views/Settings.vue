<template lang="pug">
#settings
  b-container-fluid#container
    b-row.h-100.d-flex.m-0
      b-col.pl-5#leftBlock(cols='6')
        #title.ml-5.mt-5
          img(src='../assets/tomato.png' alt='tomato')
          p Work Ringrone
        #line.ml-5
        #ringlist
          b-form-group(v-for="item in items")
            b-form-radio(name="work" :value="item" @change="selectRingtone") {{ item.name }}
        #nowPlaying
          b-btn#pause1.mx-1(variant="primary" @click="pause")
            img(src='../assets/pause1.png' alt='pause')
          #timeBar1
            b-progress(:value="40" height="10px")
          #topNo1
            p The First Thing To Do Today
            h5 09:20
      b-col.p-0(cols='6')
        #title.ml-5.mt-5
          img(src='../assets/tomato.png' alt='tomato')
          p Break Ringrone
        #line.ml-5
        #ringlist
          b-form-group(v-for="item in items")
            b-form-radio(name="break" :value="item" @change="selectRingtone") {{ item.name }}
        #group
          img(src='../assets/Group 39.png' alt='group')
</template>

<script>
export default {
  data () {
    return {
      items: [
        { name: 'Alarm', src: 'alarm.mp3' },
        { name: 'yay', src: 'yay.mp3' }
      ],
      fields: [
        { key: 'name', label: '名稱' },
        { key: 'src', label: '試聽' },
        { key: 'select', label: '選擇' }
      ]
    }
  },
  methods: {
    selectRingtone (item) {
      console.log(item.src)
      this.$store.commit('selectRingtone', item.src)
    }
  },
  computed: {
    ringtone () {
      return this.$store.state.ringtone
    }
  }
}
</script>
