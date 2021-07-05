<template>
  <div id="app">
    <start
      v-if="!isShowPopup"
      @showPopup="showPopup"
    />
    <popup
      v-else
      @showPopup="showPopup"
    />
  </div>
</template>

<script>
import popup from './components/popup.vue'
import start from './components/start.vue'
export default {
  name: 'App',
  components: {
    start,
    popup
  },
  data () {
    return {
      isShowPopup: false
    }
  },
  methods: {
    showPopup (value) {
      this.isShowPopup = value
    }
  }
}
</script>

<style lang="scss">
  
</style>
