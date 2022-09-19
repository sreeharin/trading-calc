<template>
	<div>
		<c-box w="50vw" m="auto" >
			<c-simple-grid mt="2em" :spacing="2" :columns="4">
				<c-input placeholder="Entry" w="10em" v-model="entry"></c-input>
				<c-input placeholder="Exit" w="10em" v-model="exit"></c-input>
				<c-input placeholder="Qty/Lot Size" w="10em" v-model="qty"></c-input>
				<c-input placeholder="Riskable amount" w="10em" v-model="max_risk"></c-input>
			</c-simple-grid>


			<c-stat mt="1em" color="vue.700">
				<c-stat-label color="vue.500" fontSize="lg">
					Max. allowed position size
				</c-stat-label>
				<c-stat-number v-if="this.calcPos" >
					{{this.calcPos}}
				</c-stat-number>
				<c-stat-number v-else>0</c-stat-number>
			</c-stat>
		</c-box>
	</div>
</template>

<script>
	export default{
		data(){
			return {
				entry : null,
				exit : null,
				qty : null,
				max_risk : null,
			}
		},

		computed : {
			calcPos(){
				var riskPerOne = Math.abs(this.entry - this.exit);
				return parseInt(this.max_risk/(riskPerOne*this.qty));
			},
		}
	}
</script>
