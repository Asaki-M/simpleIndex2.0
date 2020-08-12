<template>
	<div class="tianqi-kuang">
		<div class="current-city">
			当前城市：<span @click="isOpenSelectCity = !isOpenSelectCity">{{currentCity}}</span>
			<i class="iconfont icon-guanbi" @click="closeTianqiKuang"></i>
		</div>
		<select-city v-if="isOpenSelectCity" @getCurrentCity="getCurrentCity"></select-city>
		<div class="now">
			<i class="iconfont" :class="getWeatherType(weatherData[0].text_day)"></i>
			<p>{{weatherData[0].text_day}}</p>
			<p>{{weatherData[0].low}}℃ ~ {{weatherData[0].high}}℃</p>
			<p>风向: {{weatherData[0].wind_direction}}</p>
		</div>
		<div class="later">
			<div class="tomorrow">
				<p>明天</p>
				<i class="iconfont" :class="getWeatherType(weatherData[1].text_day)"></i>
				<p>{{weatherData[1].low}}℃ ~ {{weatherData[1].high}}℃</p>
			</div>
			<div class="afterTomorrow">
				<p>后天</p>
				<i class="iconfont" :class="getWeatherType(weatherData[2].text_day)"></i>
				<p>{{weatherData[2].low}}℃ ~ {{weatherData[2].high}}℃</p>
			</div>
		</div>
	</div>
</template>

<script>
	import pinyinUtil from '../assets/changeToPinyin.js'
	import selectCity from './selectCity.vue'
	export default {
		name: 'tianqi',
		data() {
			return {
				isOpenSelf: true,
				localStorage: window.localStorage,
				isOpenSelectCity: false,
				currentCity: '',
				weatherData: new Array()
			}
		},
		props: ['initData'],
		components:{
			'select-city': selectCity
		},
		created() {
			this.weatherData = this.initData
		},
		mounted() {
			this.currentCity = this.localStorage.getItem("currentCity")
			this.getWeatherData(this.currentCity)
		},
		methods:{
			closeTianqiKuang() {
				this.isOpenSelf = false
				this.$emit("changeOpen", this.isOpenSelf)
			},
			getCurrentCity(city, isClose) {
				this.isOpenSelectCity = isClose
				this.localStorage.setItem("currentCity",city)
				this.currentCity = this.localStorage.getItem("currentCity")
				this.getWeatherData(pinyinUtil.chineseToPinYin(city))
			},
			// 发送请求，获取数据
			getWeatherData(city) {
				const url = 'http://139.9.60.105:10233/weather?city=' + city
				this.$axios.get(url).then(suc => {
					this.weatherData = suc.data.results[0].daily
				}).catch(err => {
					console.log(err)
				})
			},
			// 判断天气类别使用类名
			getWeatherType(weather) {
				switch(weather) {
					case '晴':
						return 'icon-qingtian'
					case '多云':
						return 'icon-duoyun'
					case '阴':
						return 'icon-yintian'
					case '阵雨':
						return 'icon-zhenyu'
					case '雷阵雨':
						return 'icon-leizhenyu'
					case '小雨':
						return 'icon-xiaoyu'
					case '中雨':
						return 'icon-zhongyu'
					case '大雨':
						return 'icon-dayu'
					case '暴雨':
						return 'icon-dabaoyu'
					case '大暴雨':
						return 'icon-dabaoyu'
					case '特大暴雨':
						return 'icon-dabaoyu'
					case '雪':
						return 'icon-xue'
					case '雾':
						return 'icon-wu'
					default: 
						return 'icon-weizhi'
				}
			}
		}
	}
</script>

<style scoped="scoped">
	.tianqi-kuang{
		width: 15rem;
		height: 20rem;
		background: rgba(100,100,100,0.6);
		position: absolute;
		right: 0.5em;
		top: 3.5em;
		border-radius: 1em;
		animation: fade 0.8s;
	}
	@keyframes fade{
		from{opacity: 0;}
		to{opacity: 1;}
	}
	
	.current-city{
		height: 5%;
		margin-left: 1em;
		color: #ccc;
		font-size: 0.8em;
		font-weight: bold;
	}
	.current-city > span{
		font-size: 1.3em;
		color: #e6e6e6;
		cursor: pointer;
		transition: all 0.5s;
	}
	.current-city > span:hover{
		color: rgba(0,168,255,0.6);
		transition: all 0.5s; 
	}
	.current-city > i{
		position: absolute;
		top: 0.2em;
		right: 0.5em;
		cursor: pointer;
		font-size: 1.5em;
		transition: all 0.5s;
	}
	.current-city > i:hover{
		color: rgba(0,168,255,0.6);
		transition: all 0.5s; 
	}
	
	.now{
		width: 100%;
		height: 60%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		color: #e6e6e6;
		border-bottom: 1px solid #ddd;
	}
	.now > i{
		font-size: 4em;
	}
	.now > p{
		margin: 0.25em 0;
	}
	.later{
		width: 100%;
		height: 35%;
		display: flex;
		justify-content: space-around;
		align-items: center;
		color: #e6e6e6;
	}
	.later > .tomorrow, .later > .afterTomorrow {
		flex: 1;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.later > .tomorrow > i, .later > .afterTomorrow > i{
		font-size: 2em;
		padding: 0.3em 0;
	}
</style>
