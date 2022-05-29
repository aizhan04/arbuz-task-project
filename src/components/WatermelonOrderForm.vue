<template>
  <form @submit.prevent="handleSubmit">
    <label for="address">Введите адрес доставки</label>
    <input
      class="input"
      type="text"
      placeholder="Введите адрес доставки"
      id="address"
      v-model="address"
    />

    <label for="phone">Введите номер телефона</label>
    <input
      class="input"
      type="tel"
      placeholder="Введите номер телефона"
      id="phone"
      v-model="phone"
    />

    <label for="date">Выберите дату доставки</label>
    <input
      class="input"
      type="date"
      id="date"
      v-model="date"
      :min="minDate"
      :max="maxDate"
    />

    <label for="appt">Выберите время доставки</label>

    <input class="input" type="time" id="appt" name="appt" v-model="time" />
    <p v-if="!isValid" class="form-error">Заполните все поля</p>

    <button class="btn-submit">Оформить заказ</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      address: '',
      phone: '',
      time: '',
      date: '',
      isValid: true,
    };
  },
  computed: {
    minDate() {
      return new Date().toISOString().split('T')[0];
    },
    maxDate() {
      const date = new Date();
      date.setDate(date.getDate() + 9);

      return date.toISOString().split('T')[0];
    },
  },
  methods: {
    handleSubmit() {
      if (!this.address || !this.phone || !this.time || !this.date) {
        this.isValid = false;
        return;
      }

      if (!this.isValid) {
        this.isValid = true;
      }

      const formData = {
        address: this.address,
        phone: this.phone,
        time: this.time,
        date: this.date,
      };

      this.$emit('submit-form', formData);
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  align-items: center;

  border: 1px solid #ccc;
  border-radius: 5px;
  max-width: 300px;
  padding: 10px 30px;
}

.input {
  width: 100%;
  margin-top: 10px;
  margin-bottom: 20px;
  padding: 10px 15px;

  border: 2px solid rgb(149, 150, 149);
  border-radius: 5px;
  transition: all 0.3s ease;
}

.input:focus {
  border: 2px solid rgb(172, 229, 172);
  border-radius: 5px;

  outline: none;
}

label {
  display: flex;
  align-items: flex-start;
}

.form-error {
  font-size: 14px;
  line-height: 17px;
  color: red;
  margin-bottom: 10px;
}

.btn-submit {
  background-color: rgb(88, 157, 88);
  border: none;
  border-radius: 5px;
  color: white;
  padding: 10px 15px;
  cursor: pointer;

  transition: all 0.3s ease;
}

.btn-submit:hover {
  background-color: rgb(73, 129, 73);
}
</style>
