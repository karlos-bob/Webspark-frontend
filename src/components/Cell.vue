<template>
	<div class="cell" :data-cell="cellStatus">
		<p>{{ value }}</p>
		<p>
			<span class="role">{{ role }}</span>
			{{ coords }}
		</p>
	</div>
</template>

<script>
	export default {
		name: "Cell",

		props: {
			value: {
				required: true,
				type: String
			},
			coords: {
				required: true,
				type: String
			},
			role: {
				required: false,
				type: String
			}
		},

		data() {
			return {
				cellStatus: 'free'
			}
		},

		created() {
			if( this.value === '.' ) this.cellStatus = 'free'
			if( this.value === 'X' ) this.cellStatus = 'corrupted'

			// else {
			// 	console.log('Unknown or empty value!');
			// }
		}

	}
</script>

<style lang="scss" scoped>

	.cell {
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 1fr 1fr;
		width: 100%;
		height: auto;
		background-color: #fff;

		p {
			display: flex;
			align-items: center;
			justify-content: center;

			> span.role {
				font-weight: bold;
				margin-right: 3px;
				text-transform: uppercase;
			}
		}


		&[data-cell='free'] {
			background-color: #49fc09;
		}

		&[data-cell='corrupted'] {
			background-color: #e52d2d;
		}

	}

</style>