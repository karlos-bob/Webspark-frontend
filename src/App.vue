<template>
	<div id="app">

		<div class="content-wrap">

			<!-- simple visualisation of field -->
			<Field :gridList="gridList"/>

			<!-- control-buttons -->
			<div class="control">
				<!--<button @click="createField" id="create">Start</button>-->
				<button @click="minWalk" id="build">minWalk()</button>
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

				eventBus.$emit('createField');

				// todo refactor
				let gridListNumberized = this.gridList.map( row => {
					return row.replace(/\./g, '1')
						.replace('X', '0')
						.split('').map(str => str = +str);
				});

				let graph = new Graph( gridListNumberized, { diagonal: true } );
				let start = graph.grid[this.startX][this.startY];
				let end = graph.grid[this.endX][this.endY];
				let result = astar.search(graph, start, end);

				this.result = result.length;
			},
		},

		mounted() {
			eventBus.$on('minWalk', () => this.minWalk())
		}


	}
</script>

<style lang="scss">

	html, body, div, span, applet, object, iframe,
	h1, h2, h3, h4, h5, h6, p, blockquote, pre,
	a, abbr, acronym, address, big, cite, code,
	del, dfn, em, img, ins, kbd, q, s, samp,
	small, strike, strong, sub, sup, tt, var,
	b, u, i, center,
	dl, dt, dd, ol, ul, li,
	fieldset, form, label, legend,
	table, caption, tbody, tfoot, thead, tr, th, td,
	article, aside, canvas, details, embed,
	figure, figcaption, footer, header, hgroup,
	menu, nav, output, ruby, section, summary,
	time, mark, audio, video {
		margin: 0;
		padding: 0;
		border: 0;
		font-size: 100%;
		font: inherit;
		vertical-align: baseline;
	}
	/* HTML5 display-role reset for older browsers */
	article, aside, details, figcaption, figure,
	footer, header, hgroup, menu, nav, section {
		display: block;
	}
	body {
		line-height: 1;
	}
	ol, ul {
		list-style: none;
	}
	blockquote, q {
		quotes: none;
	}
	blockquote:before, blockquote:after,
	q:before, q:after {
		content: '';
		content: none;
	}
	table {
		border-collapse: collapse;
		border-spacing: 0;
	}

	#app {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		background: #20262E;
		padding: 20px;
		font-family: Helvetica;
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


						&#create {
							/*background-color: yellowgreen;*/
							background-color: dodgerblue;
						}

						&#build {
							background-color: dodgerblue;
						}
					}


				}
			}

		}
	}

</style>
