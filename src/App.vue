<template>
	<div id="app">

		<div class="content-wrap">

			<!-- simple visualisation of field -->
			<Field :gridList="gridList"/>

			<div class="control">
				<button @click="createField" id="create">Create world</button>
				<button @click="minWalk(gridList, startX, startY, endX, endY)" id="build">Count steps</button>
			</div>

			<div id="banner-message">
				<p id="result" :data-result="result === '' ? 'empty' : 'counted'">{{ result === ''? 'result...' : result }}</p>
			</div>

		</div>

	</div>
</template>

<script>
	import {eventBus} from "./main";

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

			createField() {
				eventBus.$emit('createField')
			},

			minWalk(gridList, startX, startY, endX, endY) {

				// eslint-disable-next-line no-constant-condition
				if(false) {
					alert('Тест');
				}


				console.table(
					'gridList:', gridList,
					'startX:', startX,
					'startY:',startY,
					'endX', endX,
					'endY', endY
				);


				return '';
			}
		},


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
				justify-content: space-between;
				align-items: center;
				width: 100%;
				/*margin: 30px 0;*/

				button {
					cursor: pointer;
					padding: 15px 30px;
					outline: none;
					border: none;
					font-size: 15px;
					border-radius: 4px;

					&:hover, &:active {
						transition: background-color ease .1s;

						&#create {
							background-color: yellowgreen;
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
