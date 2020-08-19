<template>
	<div class="field" v-bind:style="fieldStyle">
		<p v-if="!cellsList.length">Please, press "Start" button</p>
		<Cell v-else
			v-for="(item, index) of cellsList"
			:value="item.value"
			:coords="item.coords"
			:key="index"
			:role="item.role"
		/>
	</div>
</template>

<script>

	import {eventBus} from "../main";

	export default {
		name: "Field",

		props: {
			gridList: {
				type: Array,
				required: true
			}
		},

		components: {
			Cell: () => import('./Cell')
		},

		data() {
			return {

				// object for inline styles
				fieldStyle: {
					'grid-template-rows': '',
					'grid-template-columns': ''
				},

				// special array only used for drawing UI
				cellsList: []
			}
		},


		mounted() {
			eventBus.$on('createField', ({start, end}) => {

				// clear
				this.cellsList.length = 0;

				// .entries() was used because of using (index, value) in for...of cycle (i know about simple for() cycle :))
				for( let [rowIndex, row] of this.gridList.entries() ) {

					// make an array from string
					let arrRow = row.split("");

					for (let [valueIndex, value] of arrRow.entries()) {

						// create new cell which be an object with 2 props(coords & value)
						let cell = {};
						cell.coords = `${rowIndex}.${valueIndex}`
						cell.value = value;

						if(cell.coords === start) cell.role = 'start'
						if(cell.coords === end) cell.role = 'end'

						// fill the array which will be iterated
						this.cellsList.push(cell)
					}
				}

				// console.log('cellsList results -> ', this.cellsList);
				eventBus.$emit('cellsList', this.cellsList);

				// inline styles for component
				this.fieldStyle['grid-template-columns'] = `repeat(${this.gridList[0].length}, 1fr)`;
				this.fieldStyle['grid-template-rows'] = `repeat(${this.gridList.length}, 94px)`;

				eventBus.$emit('astar', this.cellsList)
			});
		}
	}
</script>

<style lang="scss" scoped>

	.field{
		display: grid;
		grid-gap: 2px;
		margin: 30px 0;
		width: 100%;

		p {
			width: 100%;
			color: #fff;
			margin: 30px auto;
		}
	}

</style>