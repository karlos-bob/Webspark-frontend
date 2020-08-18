<template>
	<div class="cell" :data-cell="cellStatus" :data-role="role">
		<p>{{ value }}</p>
		<p> {{ role ? role : '' }} {{ coords }}</p>
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
			else {
				console.log('Unknown value!');
			}
		}

	}
</script>

<style lang="scss" scoped>

	.cell {
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 1fr 1fr;
		/*justify-content: center;*/
		/*align-items: center;*/
		/*flex-direction: column;*/
		width: 100%;
		height: auto;
		background-color: #fff;

		p {
			display: flex;
			align-items: center;
			justify-content: center;
		}


		&[data-cell='free'] {
			background-color: #49fc09;
		}

		&[data-cell='corrupted'] {
			background-color: #e52d2d;
		}

	}

</style>