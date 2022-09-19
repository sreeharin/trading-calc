<template>
	<c-box w="50vw" m="auto" mt="1em">
		<c-stack :spacing="5" isInline justify="end" mb="2em">
			<c-text >Conventional calculator</c-text>
			<c-switch size="sm" v-on:click="switchFunc()" />
		</c-stack>

		<c-box v-if=!convCalc>
			<c-simple-grid :spacing="10" :columns="3" >
				<c-input placeholder="Capital" v-model=capital mr="1em"></c-input>
				<c-flex direction="column">
					<c-input
						:is-invalid=marginInvalid
						placeholder="Margin required" 
						v-model=margin
					>
					</c-input>
					<span 
						v-if=marginInvalid
						v-chakra 
						color="red.400"
						fontSize="sm"
						mt="0.5em"
					>
						Margin greater than capital
					</span>
				</c-flex>
				<c-select placeholder="Max. Risk" v-model="risk" >
					<option value="2">2%</option>
					<option value="1.75">1.75%</option>
					<option value="1.5">1.5%</option>
					<option value="1.25">1.25%</option>
					<option value="1">1%</option>
					<option value="0.75">0.75%</option>
					<option value="0.5">0.5%</option>
					<option value="0.25">0.25%</option>
					<option value="0.075">0.075%</option>
					<option value="0.05">0.05%</option>
					<option value="0.025">0.025%</option>
				</c-select>
			</c-simple-grid>

			<c-simple-grid :columns="2" mt="2em">
				<c-heading size="sm" color="vue.500">
					Maximum Risk
				</c-heading>
				<c-heading size="sm" color="vue.500">
					Recommended reward ({{reward}}x)
				</c-heading>
				<c-box color="vue.700">
					-{{maximumRisk}}
					<c-tooltip label="Stop loss percentage">
						({{stopLoss}}%)
					</c-tooltip>
				</c-box>
				<c-box color="vue.700">
					+{{recoReward}}
					<c-tooltip label="Target percentage">
						({{recoTarget}}%)
					</c-tooltip>
				</c-box>
			</c-simple-grid>
		</c-box>

		<c-box v-else w="75%" m="auto" >
			<c-flex justify="space-between">
				<c-box>
					<c-flex direction="column" mb="1em">
						<c-text color="vue.500">Entry price</c-text>
						<c-input placeholder="Entry price" w="15em" v-model="entry"/>
					</c-flex>
					<c-flex direction="column" mb="1em">
						<c-text color="vue.500">Exit price</c-text>
						<c-input placeholder="Exit price" w="15em" v-model="exit"/>
					</c-flex>
					<c-flex direction="column">
						<c-text color="vue.500">Target price</c-text>
						<c-input placeholder="Target price" w="15em" v-model="target"/>
					</c-flex>
				</c-box>

				<c-box>
					<c-heading size="md" color="vue.500" mb="0.5em">
						Risk : Reward
					</c-heading>
					<c-text
						color="vue.700"
					>
						1 : {{this.calcReward}}
					</c-text>
				</c-box>
			</c-flex>

		</c-box>
	</c-box>
</template>

<script >
	export default{
		data(){
			return {
				capital : null,
				margin : null,
				risk : null,
				entry : null,
				exit : null,
				target : null,
				reward : 2.5,
				marginInvalid : false,
				convCalc : false,
			}
		},
		
		computed : {
			maximumRisk(){
				return parseFloat(this.capital * (this.risk/100)).toFixed(2);
			},

			stopLoss(){
				if(this.maximumRisk == 0){
					return parseFloat(0).toFixed(2);
				}
				return parseFloat((this.maximumRisk * 100)/this.margin)
				.toFixed(2);
			},

			recoReward(){
				return parseFloat(this.maximumRisk*this.reward).toFixed(2);
			},

			recoTarget(){
				return parseFloat(this.stopLoss*this.reward).toFixed(2);
			},

			calcRisk(){
				return parseFloat(this.entry - this.exit).toFixed(2);
			},

			calcReward(){
				var tmp = ((this.target - this.entry)/this.calcRisk);
				if(isNaN(tmp)){
					return 1;
				}
				return parseFloat(tmp).toFixed(2);
			},

		},

		watch: {
			margin(value){
				if(parseFloat(this.capital) < parseFloat(value)){
					this.marginInvalid = true;
				}else{
					this.marginInvalid = false;
				}
			},
			capital(value){
				if(parseFloat(this.margin) > parseFloat(value)){
					this.marginInvalid = true;
				}else{
					this.marginInvalid = false;
				}
			},
		},

		methods : {
			switchFunc(){
				this.convCalc = !this.convCalc;
			},
		},

	}

</script>
