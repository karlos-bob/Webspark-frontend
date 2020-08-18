<template>
	<div class="field" v-bind:style="fieldStyle">
		<p v-if="!cellsList.length">Please, press "Start" button</p>
		<Cell v-else
			v-for="(item, index) of cellsList"
			:value="item.value"
			:coords="item.coords"
			:key="index"
			:role="`test`"
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
				fieldStyle: {
					'grid-template-rows': '',
					'grid-template-columns': ''
				},
				// cellsCounter: undefined,
				cellsList: []
			}
		},


		mounted() {
			eventBus.$on('createField', () => {

				// clear
				this.cellsList.length = 0;

				for( let [rowIndex, row] of this.gridList.entries() ) {

					// make an array from string
					let arrRow = row.split("");

					for (let [valueIndex, value] of arrRow.entries()) {
						// create new cell which be an object with 2 props(coords & value)
						let cell = {};

						cell.coords = `${rowIndex}.${valueIndex}`
						cell.value = value;

						// fill the array which will be iterated
						this.cellsList.push(cell)
					}
				}

				// console.log('resulted cellsList!', this.cellsList);
				eventBus.$emit('cellsList', this.cellsList);

				// inline styles for component
				this.fieldStyle['grid-template-columns'] = `repeat(${this.gridList[0].length}, 1fr)`;
				this.fieldStyle['grid-template-rows'] = `repeat(${this.gridList.length}, 94px)`;

				eventBus.$emit('astar', this.cellsList)

				// deprecated
				// this.cellsCounter = this.gridList[0].length * this.gridList.length;
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
		/*border: 1px solid #fff;*/

		p {
			width: 100%;
			color: #fff;
			margin: 30px auto;
		}
	}

</style>