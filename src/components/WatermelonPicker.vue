<template>
	<div>Выберите ваш арбуз</div>

	<div class="picker-wrapper">
		<watermelon-picker-row
			v-for="(row, idx) in watermelonRows"
			:key="idx"
			:watermelon-row="row"
			:row-number="idx"
			@pickWatermelon="HandlePickedWatermelon"
		/>
	</div>
</template>

<script>
import WatermelonPickerRow from "./WatermelonPickerRow.vue";

export default {
	components: { WatermelonPickerRow },
	name: "WatermelonPicker",
	props: {
		watermelonRows: {
			type: Array,
			required: true,
		},
	},
	mounted() {
		console.log(this.watermelonRows);
	},
	methods: {
		HandlePickedWatermelon(pickedWatermelon) {
			this.pickedWatermelon = pickedWatermelon;

			this.$emit("pickWatermelon", pickedWatermelon);
		},
	},
};
</script>

<style scoped>
.picker-wrapper {
	/* min-width: 800px;
	max-width: 1000px;
	border: 1px solid #bdbdbd;
	border-radius: 6px;
	padding: 10px; */

	display: flex;
	flex-direction: column;
	align-items: center;
	row-gap: 20px;
}
</style>
