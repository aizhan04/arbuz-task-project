<template>
  <div class="app-wrapper">
    <watermelon-picker
      :watermelon-rows="watermelonRows"
      @pickWatermelon="HandlePickedWatermelon"
      :pickedWatermelon="pickedWatermelon"
    />

    <div v-if="pickedWatermelon" class="basket">
      <h3>Ваш выбранный арбуз</h3>
      <div class="basket-info">
        <p>Арбуз №: {{ pickedWatermelon.id }}</p>
        <p>Статус арбуза: {{ pickedWatermelonStatus }}</p>
        <p>Вес арбуза: {{ pickedWatermelon.weight }}</p>
        <p>Кол-во: {{ pickedWatermelon.size }}</p>
        <p>Порезать дольками: {{ pickedWatermelon.isSliced ? 'Да' : 'Нет' }}</p>
      </div>
    </div>

    <watermelon-order-form @submit-form="handleFormSubmit" />
  </div>
</template>

<script>
import WatermelonPicker from './components/WatermelonPicker.vue';
import WatermelonOrderForm from './components/WatermelonOrderForm.vue';
import { statusToText } from './utils/status';

export default {
  components: { WatermelonPicker, WatermelonOrderForm },
  name: 'App',
  data() {
    return {
      watermelonRows: [
        [
          {
            id: 1,
            status: 'ripe',
            weight: 3,
          },
          {
            id: 2,
            status: 'unripe',
            weight: 7,
          },
          {
            id: 3,
            status: 'ripe',
            weight: 7,
          },
          {
            id: 4,
            status: 'dropped',
            weight: 10,
          },
          {
            id: 5,
            status: 'dropped',
            weight: 10,
          },
          {
            id: 6,
            status: 'dropped',
            weight: 10,
          },
          {
            id: 7,
            status: 'dropped',
            weight: 10,
          },
        ],
        [
          {
            id: 8,
            status: 'unripe',
            weight: 8,
          },
          {
            id: 9,
            status: 'unripe',
            weight: 6,
          },
          {
            id: 10,
            status: 'dropped',
            weight: 7,
          },
          {
            id: 11,
            status: 'ripe',
            weight: 9,
          },
          {
            id: 12,
            status: 'ripe',
            weight: 9,
          },
          {
            id: 13,
            status: 'ripe',
            weight: 9,
          },
        ],
      ],
      pickedWatermelon: null,
      formInfo: null,
      isValid: true,
    };
  },
  computed: {
    pickedWatermelonStatus() {
      return statusToText[this.pickedWatermelon.status];
    },
  },
  methods: {
    HandlePickedWatermelon(pickedWatermelon) {
      this.pickedWatermelon = pickedWatermelon;
    },
    handleInfo(formInfo) {
      this.formInfo = formInfo;
    },
    handleFormSubmit(data) {
      if (!this.pickedWatermelon) {
        alert('Вы не выбрали арбуз');
        return;
      }

      const payload = {
        ...data,
        ...this.pickedWatermelon,
      };

      console.log(payload);
      alert('Ваш заказ успешно оформлен');
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 30px;
}

.app-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  row-gap: 20px;
}

h3 {
  margin-bottom: 5px;
}

.basket {
  background-color: #cccc;
  padding: 10px 30px;
  border-radius: 6px;
  max-width: 300px;
}

.basket-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
</style>
