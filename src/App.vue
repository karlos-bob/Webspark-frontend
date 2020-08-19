<template>
	<div id="app">

		<div class="content-wrap">

			<!-- simple visualisation of field -->
			<Field :gridList="gridList"/>

			<!-- control-buttons -->
			<div class="control">
				<!--<button @click="reset" id="reset">Reset</button>-->
				<button @click="minWalk" id="minWalk">minWalk()</button>
			</div>

			<!-- message with result -->
			<div id="banner-message">
				<p id="result" :data-result="result === '' ? 'empty' : 'counted'">
					{{ result === ''? 'result...' : result }}
				</p>
			</div>

		</div>

	</div>
</template>

<script>

	import { eventBus } from "./main";
	import { astar, Graph } from './astar'

	export default {
		name: 'App',

		components: {
			Field: () => import("./components/Field")
		},

		data() {
			return {
				gridList: [
					'.X.',
					'.X.',
					'...',
				],
				startX: 2,
				startY: 1,
				endX: 0,
				endY: 2,

				result: ''
			}
		},

		methods: {

			minWalk() {

				// draw UI field
				eventBus.$emit('createField', {
					start: `${this.startX}.${this.startY}`,
					end: `${this.endX}.${this.endY}`
				});

				if(this.gridList.every( row => row.length > 1 && row.length < 100)) {

					// make new array with arrays of numbers
					let gridListNumberized = this.gridList.map( row => {
						return row
							.replace(/\./g, '1')
							.replace('X', '0')
							.split('')
							.map(str => str = +str);
					});

					let graph = new Graph( gridListNumberized, { diagonal: true } );
					let start = graph.grid[this.startX][this.startY];
					let end = graph.grid[this.endX][this.endY];
					let result = astar.search(graph, start, end);

					// write final result to data
					this.result = result.length;
				} else {
					alert('gridList row must be > 1 and < 100!');
				}
			},

			reset() {
				// todo reset app
			}
		},

		mounted() {
			eventBus.$on('minWalk', () => this.minWalk())
		}

	}
</script>

<style lang="scss">
	@import "./assets/reset";

	#app {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		background: #20262E;
		padding: 20px;
		font-family: Helvetica, sans-serif;
		height: 100vh;

		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.content-wrap {
			display: flex;
			flex-direction: column;
			align-items: center;

			#banner-message {
				background: #fff;
				border-radius: 4px;
				padding: 20px;
				font-size: 25px;
				text-align: center;
				transition: all 0.2s;
				margin: 30px auto 0;
				width: 300px;

			}

			p#result {
				&[data-result='empty'] {
					color: rgba(128, 128, 128, 0.88);
				}

				&[data-result='counted'] {
					color: #000;
				}
			}

			.control {
				display: flex;
				flex-direction: row;
				justify-content: stretch;
				align-items: center;
				width: 100%;
				/*margin: 30px 0;*/

				button {
					cursor: pointer;
					padding: 15px 30px;
					outline: none;
					border: none;
					font-size: 18px;
					border-radius: 4px;
					width: 100%;

					&:hover, &:active {
						transition: background-color ease .1s;
						color: #fff;


						&#reset {
							background-color: #ff1e6d;
						}

						&#minWalk {
							background-color: dodgerblue;
						}
					}


				}
			}

		}
	}

</style>
