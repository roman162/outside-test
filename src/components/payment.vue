<template>
  <div class="early-payment">
    <div class="early-payment__checkbox">
      <span
        class="early-payment__checkbox-container"
        :class="isChecked ? 'early-payment__checkbox-container--active' : ''"
        @click="setCheckbox"
      >
        <svg
          class="early-payment__checkbox-icon"
          width="14"
          height="11"
          viewBox="0 0 14 11"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M4.45455 8.70149L1.11364 5.25373L0 6.40299L4.45455 11L14 1.14925L12.8864 0L4.45455 8.70149Z" fill="white"/>
        </svg>
      </span>
    </div>
    <span class="early-payment__sum">
      {{ sum }}
      <span class="early-payment__year">{{ getYear }}</span>
    </span>
  </div>
</template>

<script>
export default {
  name: 'payment',

  props: {
    deduction: {
      type: Object
    },
    index: {
      type: Number
    },
    year: {
      type: String,
      default: '-ый год'
    }
  },
  data () {
    return {
      isChecked: false
    }
  },
  computed: {
    sum () {
      return this.deduction.deduction + ' рублей '
    },
    getYear () {
      return (this.index === 1 ? 'во ' : 'в ') + (this.index + 1) + this.year
    }
  },
  mounted () {
    this.isChecked = this.deduction.checked
  },
  methods: {
    setCheckbox () {
      this.isChecked = !this.isChecked
      this.$emit('isChecked', {
        index: this.index,
        checked: this.isChecked
      })
    }
  }
}
</script>

<style scoped lang="scss">
  .early-payment{
    display: flex;
    padding: 18px 0;
    border-bottom: 1px solid #DFE3E6;
  }

  .early-payment__checkbox-container{
    margin-right: 11px;
    width: 20px;
    height: 20px;
    border-radius: 6px;
    border: 1px solid #DFE3E6;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;

    &--active{
      border: none;
      background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
    }
  }
</style>