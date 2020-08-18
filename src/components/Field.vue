<template>
	<div class="field" v-if="cellsList.length" v-bind:style="fieldStyle">
		<Cell v-for="(item, index) of cellsList" :key="index" :value="item">
			<template #coordinates>{{  }}</template>
		</Cell>
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

		// created() {
		// 	console.log(this.gridList);
		// },

		mounted() {
			eventBus.$on('createField', () => {

				// todo make check for equal amount of items in gridList
				// todo temp


				for( let row of this.gridList) { this.cellsList.push(...row.split("")) }

				console.log('cellsList', this.cellsList);

				this.fieldStyle['grid-template-columns'] = `repeat(${this.gridList[0].length}, 60px)` ;
				this.fieldStyle['grid-template-rows'] = `repeat(${this.gridList.length}, 60px)` ;

				// deprecated
				// this.cellsCounter = this.gridList[0].length * this.gridList.length;
			})
		}
	}
</script>

<style lang="scss" scoped>

	.field{
		display: grid;
		border: 1px solid #fff;
		grid-gap: 1px;
		margin: 30px 0;
	}

</style>