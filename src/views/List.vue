<template lang="pug">
#list
  b-container-fluid#container
    b-row.h-100.d-flex.m-0
      b-col.pl-5#leftBlock(cols='6')
        #title.ml-5.mt-5
          img(src='../assets/tomato.png' alt='tomato')
          p Done
        #line.ml-5
        b-table-simple#table(striped hover)
          tbody
            tr(v-for="(item, idx) in finished")
              td
                img(src='../assets/checked.png' alt='checked')
              td {{ item }}
              td
                b-btn#del(variant="outline-danger" @click="delfinish(idx)")
                  img(src='../assets/cancel.png' alt='skip')
            tr(v-if="finished.length === 0")
              td.text-center(colspan="2") No Completed Mission
        #nowPlaying
          b-btn#pause1.mx-1(variant="primary" @click="pause")
            img(src='../assets/pause1.png' alt='pause')
          #timeBar1
            b-progress(:value="40" height="10px")
          #topNo1
            p The First Thing To Do Today
            h5 09:20
      b-col#rightBlock.p-0(cols='6')
        #title.ml-5.mt-5
          img(src='../assets/tomato.png' alt='tomato')
          p Focus Time
        #line.ml-5
        img#date(src='../assets/date.png' alt='date')
        #title.ml-5.mt-5
          img(src='../assets/tomato.png' alt='tomato')
          p Chart
        #line.ml-5
        img#chart(src='../assets/chart01.png' alt='date')
</template>

<script>
export default {
  data () {
    return {
      newinput: '',
      fields: [
        { key: 'name', label: '名稱' },
        { key: 'action', label: '操作' }
      ]
    }
  },
  computed: {
    newinputstate () {
      return this.newinput.length > 2 ? true : this.newinput.length === 0 ? null : false
    },
    items () {
      return this.$store.state.items.map(item => {
        item.state = item.model.length > 2
        return item
      })
    },
    finished () {
      return this.$store.state.finished
    }
  },
  methods: {
    additem () {
      if (this.newinput.length > 2) {
        this.$store.commit('additem', this.newinput)
        this.newinput = ''
      }
    },
    edititem (index) {
      this.$store.commit('edititem', index)
    },
    delitem (index) {
      this.$store.commit('delitem', index)
    },
    submitedit (index) {
      if (this.items[index].state) {
        this.$store.commit('submitedit', index)
      }
    },
    canceledit (index) {
      this.$store.commit('canceledit', index)
    },
    delfinish (index) {
      this.$store.commit('delfinish', index)
    }
  }
}
</script>
