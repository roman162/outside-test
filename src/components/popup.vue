<template>
  <div class="popup__container">
    <div class="popup">
      <span
        class="popup__close"
        @click="showPopup"
      >
        <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M7.6151 6.00057L11.6514 1.96311C11.7605 1.85778 11.8475 1.73179 11.9073 1.59248C11.9671 1.45318 11.9986 1.30335 12 1.15174C12.0013 1.00013 11.9724 0.849774 11.915 0.709449C11.8576 0.569124 11.7728 0.441638 11.6656 0.33443C11.5584 0.227222 11.4309 0.142438 11.2905 0.0850268C11.1502 0.0276153 10.9999 -0.00127433 10.8483 4.31116e-05C10.6967 0.00136056 10.5468 0.032859 10.4075 0.0927004C10.2682 0.152542 10.1422 0.239527 10.0369 0.348583L5.99943 4.3849L1.96311 0.348583C1.85778 0.239527 1.73179 0.152542 1.59248 0.0927004C1.45318 0.032859 1.30335 0.00136056 1.15174 4.31116e-05C1.00013 -0.00127433 0.849774 0.0276153 0.709449 0.0850268C0.569124 0.142438 0.441638 0.227222 0.33443 0.33443C0.227222 0.441638 0.142438 0.569124 0.0850268 0.709449C0.0276153 0.849774 -0.00127433 1.00013 4.31116e-05 1.15174C0.00136056 1.30335 0.032859 1.45318 0.0927004 1.59248C0.152542 1.73179 0.239527 1.85778 0.348583 1.96311L4.3849 5.99943L0.348583 10.0369C0.239527 10.1422 0.152542 10.2682 0.0927004 10.4075C0.032859 10.5468 0.00136056 10.6967 4.31116e-05 10.8483C-0.00127433 10.9999 0.0276153 11.1502 0.0850268 11.2905C0.142438 11.4309 0.227222 11.5584 0.33443 11.6656C0.441638 11.7728 0.569124 11.8576 0.709449 11.915C0.849774 11.9724 1.00013 12.0013 1.15174 12C1.30335 11.9986 1.45318 11.9671 1.59248 11.9073C1.73179 11.8475 1.85778 11.7605 1.96311 11.6514L5.99943 7.6151L10.0369 11.6514C10.1422 11.7605 10.2682 11.8475 10.4075 11.9073C10.5468 11.9671 10.6967 11.9986 10.8483 12C10.9999 12.0013 11.1502 11.9724 11.2905 11.915C11.4309 11.8576 11.5584 11.7728 11.6656 11.6656C11.7728 11.5584 11.8576 11.4309 11.915 11.2905C11.9724 11.1502 12.0013 10.9999 12 10.8483C11.9986 10.6967 11.9671 10.5468 11.9073 10.4075C11.8475 10.2682 11.7605 10.1422 11.6514 10.0369L7.6151 5.99943V6.00057Z" fill="#EA0029"/>
        </svg>
      </span>
      <h3 class="popup__title">
        Налоговый вычет
      </h3>
      <p class="popup__description">
        Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13% от своего официального годового дохода.
      </p>
      <div class="popup__salary">
        <span class="popup__heading">
          Ваша зарплата в месяц
        </span>
        <input
          type="text"
          class="popup__input"
          :class="inputRequired ? 'popup__input--error' : ''"
          placeholder="Введите данные"
          required
          v-model="input"
        >
        <span
          v-if="inputRequired"
          class="popup__input-error"
        >
          {{ inputRequired === 'required' ? 'Поле обязательно для заполнения' : 'Введите число' }}
        </span>
        <button
          type="submit"
          class="popup__salary-button"
          @click="result"
        >
          Рассчитать
        </button>
      </div>
      <div
        v-if="deductions"
        class="payments"
      >
        <span class="popup__heading">Итого можете внести в качестве досрочных:</span>
        <ul class="early__payments">
          <li
            v-for="(deduction, index) in deductions"
            :key="index"
          >
            <payment
              :deduction="deduction"
              :index="index"
              :year="getYear(index)"
              @isChecked="deductionChecked"
            />
          </li>
        </ul>
      </div>
      <div class="popup__result">
        <span class="popup__heading popup__result-title">Что уменьшаем?</span>
        <div class="popup__result-buttons">
          <button class="popup__result-button"
            :class="isDecrease === 'payment' ? 'popup__result-button--active' : ''"
            @click="isDecrease = 'payment'"
          >Платеж</button>
          <button
            class="popup__result-button"
            :class="isDecrease === 'term' ? 'popup__result-button--active' : ''"
            @click="isDecrease = 'term'"
          >Срок</button>
        </div>
      </div>
      <button
        class="popup__button"
        @click="inputValidate"
      >Добавить</button>
    </div>
  </div>
</template>

<script>
import payment from './payment'
export default {
  components: {
    payment
  },
  data () {
    return {
      isDecrease: 'payment',
      input: '',
      deductions: null,
      inputRequired: null
    }
  },
  methods: {
    getYear (index) {
      const  years = [
        '-ый год',
        '-ой год',
        '-ий год',
        '-ый год',
        '-ый год',
        '-ой год',
        '-ой год',
        '-ой год',
        '-ый год',
      ]
      if (index > 8) {
        return '-ый год'
      } else {
        return years[index]
      }
    },
    showPopup () {
      this.$emit('showPopup', false)
    },
    result () {
      if (this.inputValidate()) {
        const totalDeduction = 260000
        const yearlyDeduction = Number(this.input) * 12 * 0.13
        let sum = 0
        let index = 0
        const resultDeduction = []
        while (sum < totalDeduction) {
          resultDeduction[index] = {
            deduction: yearlyDeduction,
            index: index,
            checked: true
          }
          sum += yearlyDeduction
          index++
        }
        resultDeduction[resultDeduction.length - 1].deduction = totalDeduction - yearlyDeduction * (resultDeduction.length - 1)
        this.deductions = resultDeduction
      }
    },
    deductionChecked (data) {
      this.deductions[data.index].checked = data.checked
    },
    inputValidate() {
      if (this.input.length < 1) {
        this.inputRequired = 'required'
        return false
      }
      if (isNaN(this.input)) {
        this.inputRequired = 'number'
        return false
      }
      this.inputRequired = null
      return true
    }
  }
}
</script>

<style scoped lang="scss">
  .popup__container{
    width: 100vw;
    min-height: 100vh;
    background: rgba(0, 0, 0, 0.3);
    padding-bottom: 40px;

    @media screen and (min-width: 768px){
      padding-top: 5%;
    }
  }

  .popup{
    position: relative;
    width: 100%;
    min-height: 100vh;
    padding: 16px;
    padding-top: 32px;
    font-size: 14px;
    line-height: 24px;
    display: flex;
    flex-direction: column;
    background-color: #fff;

    @media screen and (min-width: 768px) {
      min-height: auto;
      max-width: 453px;
      margin: 0 auto;
      border-radius: 30px;
    }
  }

  .popup__close{
    top: 18px;
    right: 18px;
    width: 20px;
    height: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    cursor: pointer;
  }

  .popup__title{
    margin: 0;
    font-size: 18px;
    line-height: 24px;
    margin-bottom: 16px;
    font-weight: 500;

    @media screen and (min-width: 768px){
      font-size: 28px;
      line-height: 40px;
    }
  }

  .popup__description{
    color: #808080;
    font-size: 12px;
    line-height: 16px;

    @media screen and (min-width: 768px){
      font-size: inherit;
      line-height: inherit;
    }
  }

  .popup__heading{
    margin-top: 24px;
    font-weight: 500;
  }

  .popup__salary{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .popup__input{
    border: 1px solid #DFE3E6;
    border-radius: 3px;
    padding: 8px 10px;
    margin: 8px 0;
    width: 100%;

    &:hover{
      border-color: #000;
    }

    &:focus{
      box-shadow: none;
      outline: none;
    }

    &--error{
      border-color: #EA0029;
    }
  }

  .popup__input-error{
    font-size: 10px;
    line-height: 6px;
    color: #EA0029;
    margin-bottom: 10px;
  }

  .popup__salary-button{
    background-color: inherit;
    border: none;
    display: inline;
    width: auto;
    cursor: pointer;
    color: #EA0029;
    padding: 0;
    font-weight: 500;

    &:hover{
      color: #F53A31;
    }

    &:active{
      color: #EA0029;
    }
  }

  .popup__result{
    margin-top: 24px;
    margin-bottom: 40px;

    @media screen and (min-width: 768px) {
      display: flex;
      align-items: center;
    }
  }

  .popup__result-title{
    @media screen and (min-width: 768px){
      margin-top: 0;
    }
  }

  .popup__result-buttons{
    margin-top: 24px;

    @media screen and (min-width: 768px){
      margin-top: 0;
      margin-left: 76px;
    }
  }

  .popup__result-button{
    padding: 6px 12px;
    background: #EEF0F2;
    border-radius: 50px;
    margin-right: 8px;

    &:hover{
      background: #DFE3E6;
    }

    &--active{
      color: #fff;
      background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;

      &:hover{
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
      }
    }
  }

  .popup__button{
    background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
    box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
    border-radius: 6px;
    color: #fff;
    padding: 1em;
    display: flex;
    margin-top: auto;
    justify-content: center;
  }

  .payments{
    margin-top: 16px;
  }

  .early__payments{
    list-style: none;
    padding: 0;
  }
</style>