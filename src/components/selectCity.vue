<template>	
	<div class="select-city">
		<div class="province-contain">
			<i class="iconfont icon-chengshi"></i>
			<div class="province" v-for="(province,index) in cityNames" :class="{active:index===currentProvince}" @click="getCurrentProvince(index)">
				{{province[0]}}
			</div>
		</div>
		<div class="city-contain">
			<div class="city" v-for="city in cityNames[currentProvince]" @click="sendCurrentCity(city)">
				{{city}}
			</div>
		</div>
	</div>
</template>

<script>
	import cityNames from '../assets/weatherApiCity.js'
	export default {
		name: 'selectCity',
		data() {
			return {
				cityNames: cityNames,
				currentProvince: 0,
				isOpen: true
			}
		},
		methods:{
			getCurrentProvince(pos) {
				this.currentProvince = pos
			},
			sendCurrentCity(city) {
				this.isOpen = false
				this.$emit("getCurrentCity", city, this.isOpen)
			}
		}
	}
</script>

<style scoped="scoped">
	.select-city{
		position: absolute;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		border-radius: 1em;
		display: flex;
		animation: fade 0.8s;
	}
	@keyframes fade{
		from{opacity: 0;}
		to{opacity: 1;}
	}
	
	.province-contain{
		flex: 1;
		height: 100%;
		text-align: center;
		background-color: #ddd;
		border-top-left-radius: 1em;
		border-bottom-left-radius: 1em;
		overflow-y: scroll;
	}
	.province-contain::-webkit-scrollbar{
		width: 0.3em;
	}
	.province-contain::-webkit-scrollbar-thumb{
		border-radius: 1em;
		background-color: rgba(144,147,153,.3);
	}
	.province-contain::-webkit-scrollbar-thumb:hover{
		background-color: rgba(144,147,153,.6);
	}
	.province-contain > i{
		font-size: 1.5em;
		color: #333;
		text-align: center;
		font-weight: bold;	
		transition: all 0.5s;
		display: block;
		padding: 0.2em 0; 
	}
	.province-contain > i:hover{
		cursor: pointer;
		color: rgba(0,168,255,0.6);
		transition: all 0.5s; 
	}
	.province-contain > .province{
		width: 100%;
		height: 2em;
		line-height: 2em;
		font-size: 0.9em;
		text-align: center;
		cursor: pointer;
		transition: all 0.5s;
	}
	.province-contain > .province:hover{
		background: #fefefe;
		transition: all 0.5s; 
	}
	.province-contain > .province.active{
		background-color: #fefefe;
		transition: all 0.5s;
	}
	
	
	.city-contain{
		flex: 3;
		padding-top: 1.5em;
		background-color: #fefefe;
		border-top-right-radius: 1em;
		border-bottom-right-radius: 1em;
		display: flex;
		flex-wrap: wrap;
		overflow-y: scroll;
	}
	.city-contain::-webkit-scrollbar{
		width: 0.3em;
	}
	.city-contain::-webkit-scrollbar-thumb{
		border-radius: 1em;
		background-color: rgba(144,147,153,.3);
	}
	.city-contain::-webkit-scrollbar-thumb:hover{
		background-color: rgba(144,147,153,.6);
	}
	.city{
		width: 50%;
		height: 2em;
		font-size: 0.8em;
		text-align: center;
		line-height: 2em;
		border-radius: 1em;
		cursor: pointer;
		transition: all 0.5s;
	}
	.city:hover{
		background: rgba(200,200,200,0.6);
		transition: all 0.5s;
	}
</style>
