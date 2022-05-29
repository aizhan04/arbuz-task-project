<template>
  <div class="watermelon" :class="classes" @click="isActive = !isActive">
    {{ watermelon.status === 'dropped' ? 'x' : '' }}

    <div
      class="watermelon-info"
      :class="{ 'info-active': isActive }"
      @click.stop
    >
      <p>Ряд: {{ row + 1 }}</p>
      <p>Место: {{ place + 1 }}</p>
      <p>Статус: {{ watermelonStatus }}</p>
      <p>Вес: {{ watermelon.weight }} кг</p>
      <p>Кол-во</p>

      <div class="watermelon-quantity">
        <button @click="minusWatermelon" class="minus-btn">-</button>
        <input type="number" class="quantity" min="1" max="3" v-model="size" />
        <button @click="addWatermelon" class="plus-btn">+</button>
      </div>

      <div class="slice-div">
        <input
          type="checkbox"
          id="slice"
          name="slice"
          @change="HandleCheckbox"
          :class="[isSliced ? 'checked' : '']"
        />
        <label class="slice-label" for="slice">Порезать дольками</label>
      </div>

      <button class="add-to-basket" @click="handleWatermelonPick">
        Добавить в корзину
      </button>
    </div>
  </div>
</template>

<script>
import { statusToText } from '../utils/status';

export default {
  data() {
    return {
      size: 1,
      isSliced: false,
      isActive: false,
    };
  },
  name: 'Watermelon',
  props: {
    watermelon: {
      type: Object,
      required: true,
    },
    place: {
      type: Number,
      required: true,
    },
    row: {
      type: Number,
      required: true,
    },
  },
  computed: {
    watermelonStatus() {
      return statusToText[this.watermelon.status];
    },
    classes() {
      return {
        'w-dropped': this.watermelon.status === 'dropped',
        'w-ripe': this.watermelon.status === 'ripe',
        'w-unripe': this.watermelon.status === 'unripe',
      };
    },
  },
  methods: {
    handleWatermelonPick() {
      if (this.watermelon.status === 'dropped') {
        alert('нельзя сорвать уже сорван');
        return;
      }

      let pickedWatermelon = {
        ...this.watermelon,
        place: this.place + 1,
        row: this.row + 1,
        size: this.size,
        isSliced: this.isSliced,
      };

      this.$emit('pickWatermelon', pickedWatermelon);
      this.isActive = false;
    },

    addWatermelon() {
      if (this.size >= 3) {
        this.size = 3;
      } else {
        this.size += 1;
      }
    },
    minusWatermelon() {
      if (this.size <= 1) {
        this.size = 1;
      } else {
        this.size -= 1;
      }
    },
    HandleCheckbox() {
      this.isSliced = !this.isSliced;
    },
  },
};
</script>

<style scoped>
.watermelon {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  border: 2px solid #bdbdbd;
  background-color: #f8f8f8;
  cursor: pointer;

  display: flex;
  align-items: center;
  justify-content: center;

  color: #333;
  position: relative;
}

.w-dropped {
  border-color: #ccc;
  color: #ccc;
}

.w-ripe {
  border: none;
  background-color: rgb(106, 209, 106);
}

.w-unripe {
  border: none;
  background-color: rgb(237, 189, 100);
}

.watermelon-info {
  display: none;
  position: absolute;
  top: 110%;
  right: 0;
  left: 0;
  z-index: 10;
  background-color: #fff;
  border-radius: 5px;
  flex-direction: column;
  align-items: flex-start;
  row-gap: 2px;
  min-width: 200px;
  padding: 10px 15px;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.info-active {
  display: flex;
}

.watermelon-info p {
  font-size: 14px;
  line-height: 16px;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  /* display: none; <- Crashes Chrome on hover */
  -webkit-appearance: none;
  margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
}
.quantity {
  width: 30px;
  text-align: center;
}
.watermelon-quantity {
  display: flex;
}
.minus-btn,
.plus-btn {
  background-color: rgb(88, 157, 88);
  border: none;
  border-radius: 50%;
  color: white;
  padding: 3px 8px;
  cursor: pointer;
}

.add-to-basket {
  background-color: rgb(88, 157, 88);
  border: none;
  border-radius: 5px;
  color: white;
  padding: 3px 8px;
  cursor: pointer;
  margin-top: 8px;
}

.slice-div {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.slice-div input {
  margin-right: 5px;
}
</style>
