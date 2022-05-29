<template>
	<div class="watermelon-row">
		<watermelon
			v-for="(item, idx) in watermelonRow"
			:watermelon="item"
			:key="item.id"
			:place="idx"
			:row="rowNumber"
			@pickWatermelon="HandlePickedWatermelon"
		/>
	</div>
</template>

<script>
import Watermelon from "./Watermelon.vue";

export default {
	components: { Watermelon },
	name: "WatermelonPickerRow",
	props: {
		watermelonRow: {
			type: Array,
			required: true,
		},
		rowNumber: {
			type: Number,
			required: true,
		},
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
.watermelon-row {
	display: flex;
	align-items: center;
	column-gap: 15px;
}
</style>
