<template>
  <div id="app">
    <div class="contain" ref="contain">
    	<div class="gongneng">
			<div class="huanfu" @click="isToggleBizhi = !isToggleBizhi"><i class="iconfont icon-yifu"></i></div>
			<div class="tianqi" @click="isToggleTianqi = !isToggleTianqi"><i class="iconfont icon-tianqi"></i></div>
		</div>
		<tianqi v-if="isToggleTianqi" :isToggleTianqi="isToggleTianqi" :initData="weatherData" @changeOpen="updateTianqikuang"></tianqi>
    	<bizhi v-if="isToggleBizhi" :isToggleBizhi="isToggleBizhi" class="bizhiWin" :class="{active:isToggleBizhi}" @changeOpen="updateBizhikuang" @setBgSrc="getBgSrc"></bizhi>
    	<div class="neirong">
    		<div class="sousuo">
    			<input type="text" placeholder="输入搜索内容" autocomplete="off" v-model="keywords"  @keyup.enter="search"/>
    			<button @click="search" >
    				<i class="iconfont icon-sousuo"></i>
    			</button>
    		</div>
    		<div class="sousuo-yingqin">
    			<div class="yinqin active" @click="toggleYinqin('baidu')" ref="baidu">
    				<img src="searchLogo/baidu.png" />
    			</div>
    			<div class="yinqin" @click="toggleYinqin('sougou')" ref="sougou">
    				<img src="searchLogo/sougou.png" />
    			</div>
    		</div>
			<cywang></cywang>
    	</div>
		<div class="footer">
			© 2020 | Asaki-M
		</div>
    </div>
  </div>
</template>

<script>
import pinyinUtil from './assets/changeToPinyin.js'
import bizhiKuang from './components/bizhikuang.vue'
import tianqiKuang from './components/tianqikuang.vue'
import cywangzhi from './components/cywangzhi.vue'
export default {
  name: 'App',
  data() {
	  return {
		  isToggleBizhi: false,
		  isToggleTianqi: false,
		  yinqin: 'baidu',
		  weatherData: [],
		  keywords: '',
		  storage: ''
	  }
  },
  components: {
	  "bizhi": bizhiKuang,
	  "tianqi": tianqiKuang,
	  "cywang": cywangzhi
  },
  created() {
	const storage = this.storage = window.localStorage
	if (storage.getItem("bgSrc")) {
		this.$nextTick(()=>{
			this.$refs.contain.style.backgroundImage = storage.getItem("bgSrc")
		})		
	} else{
		storage.setItem("bgSrc", "url(indexBg/anime-people-6.jpg)")
		this.$nextTick(()=>{
			this.$refs.contain.style.backgroundImage = storage.getItem("bgSrc")
		})	
	}
  },
  mounted() {
	if ( !window.localStorage.getItem("currentCity")) {
		window.localStorage.setItem("currentCity","北京")
	}
	const city = window.localStorage.getItem("currentCity") 
  	const url = 'http://139.9.60.105:10233/weather?city=' + pinyinUtil.chineseToPinYin(city)
  	this.$axios.get(url).then(suc => {
		console.log(suc)
  		this.weatherData = suc.data.results[0].daily
  	}).catch(err => {
  		console.log(err)
  	})
  },
  methods: {
	  updateBizhikuang(e) {
		  this.isToggleBizhi = e
	  },
	  updateTianqikuang(e) {
		  this.isToggleTianqi = e
	  },
	  toggleYinqin(type) {
		  this.yinqin = type
		  if (type == "baidu") {
			  this.$refs.sougou.classList.remove("active")
			  this.$refs.baidu.classList.add("active")
		  } else{
			  this.$refs.baidu.classList.remove("active")
			  this.$refs.sougou.classList.add("active")
		  }
	  },
	  search() {
		  let url = ""
		  if (this.yinqin == "baidu") {
		  	url = "https://www.baidu.com/s?ie=UTF-8&wd=" + this.keywords
			window.open(url, '_self')
		  } else{
		  	url = "https://www.sogou.com/web?query=" + this.keywords
			window.open(url, '_self')
		  }
	  },
	  getBgSrc(background) {
		  const src = "url("+background+")"
		  this.storage.setItem("bgSrc", src)
		  this.$refs.contain.style.backgroundImage = this.storage.getItem("bgSrc")
		  this.isToggleBizhi = false
	  }
  }
}
</script>

<style scoped="scoped">
.contain{
	width: 100vw;
	height: 100vh;
	font: 16px -apple-system,BlinkMacSystemFont,Helvetica Neue,Helvetica,Arial,PingFang SC,Hiragino Sans GB,Microsoft YaHei,sans-serif;
	background-size: cover;
	background-attachment: fixed;
}

.gongneng{
	position: absolute;
	top: 0;
	right: 0;
	display: flex;
	justify-content: space-around;
	align-items: center;
	width: 8em;
	height: 3em;
}
.huanfu, .tianqi{
	width: 3em;
	height: 3em;
	color: #eee;
	text-align: center;
	line-height: 3em;
	border-radius: 0.5em;
	background: rgba(100,100,100,0.6);
	cursor: pointer;
    user-select:none;
}
.huanfu > i, .tianqi > i{
	font-size: 1.5em
}

.huanfu:hover, .tianqi:hover{
	background: rgba(0,168,255,0.6);
	color: #fff;
	transition: all 0.5s;
}

.huanfu:active, .tianqi:active{
	background-color: rgba(0,0,0,.7);
	transition: all 0.5s;
}

.neirong{
	width: 100%;
	height: 95%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.sousuo{
	height: 3em;
	display: flex;
	justify-content: center;
	align-items: center;
}

.sousuo > input{
	width: 30vw;
	height: 100%;
	padding: 0 1em;
	border: none;
	outline: none;
	background: rgba(233,233,233,0.6);
	border-top-left-radius: 1em;
	border-bottom-left-radius: 1em;
	transition: all 0.5s;
}
.sousuo > input:focus{
	background: rgb(255,255,255);
	transition: all 0.5s;
}

.sousuo > button {
	width: 5vw;
	height: 100%;
	border: none;
	outline: none;
	background: rgba(233,233,233,0.6);
	border-top-right-radius: 1em;
	border-bottom-right-radius: 1em;
	cursor: pointer;
	transition: all 0.5s;
}

.sousuo > button:hover{
	background: rgba(233,233,233,0.8);
	transition: all 0.5s;
}
.sousuo > button:active{
	background: rgba(233,233,233,0.9);
	transition: all 0.3s;
}

.sousuo-yingqin{
	display: flex;
	justify-content: space-around;
	align-items: center;
	width: 30vw;
	height: 2em;
	margin-top: 2em;
}

.sousuo-yingqin > .yinqin{
	display: flex;
	justify-content: center;
	align-items: center;
	width: 3em;
	height: 1em;
	padding: 0.5em;
	border-radius: 0.5em;
	background: rgba(100,100,100,0.6);
	cursor: pointer;
	transition: all 0.7s;
}

.sousuo-yingqin > .yinqin:hover{
	background: #00a8ff;
	transition: all 0.7s;
}

.sousuo-yingqin > .yinqin.active{
	background: rgba(200,200,200,0.6);
	transition: all 0.7s;
}

.sousuo-yingqin > .yinqin > img{
	height: 1.5em;
}

.bizhiWin.active{
	animation: fadeIn 0.8s;
}

@keyframes fadeIn{
	from{
		opacity: 0;
	}
	to{
		opacity: 1;
	}
}

.footer{
	height: 5%;
	width: 100%;
	color: #ccc;
	text-align: center;
	display: flex;
	justify-content: center;
	align-items: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
</style>
