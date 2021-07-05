<template>
  <div
    class="start-window"
  >
    <button
      class="start-window__button"
      @click="showPopup"
    >
      Налоговый вычет
    </button>
  </div>
</template>

<script>
export default {
  methods: {
    showPopup () {
      this.$emit('showPopup', true)
    }
  }
}
</script>

<style scoped lang="scss">
  .start-window{
    font-size: 12px;
    overflow: hidden;
    display: flex;
    width: 100vw;
    height: 100vh;
    justify-content: center;
    align-items: center;
    background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;

     
  }

  .start-window__button{
    font-size: 12px;
    border: 1px solid #FFFFFF;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #FFFFFF;
    background-color: transparent;
    padding: 1em 2em;
    font-weight: 500;
    cursor: pointer;

    @media screen and (min-width: 768px) {
      font-size: 16px;
    }

    &:hover{
      background-color: #fff;
      color: #000;
    }

    &--disabled{
      background-color: #BEC5CC;
      color: #fff;
    }
  }
</style>