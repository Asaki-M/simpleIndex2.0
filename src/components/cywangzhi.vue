<template>
	<div class="cy-contain">
		<div class="cy-web" v-for="(item,index) in cywebs" :key="this">
			<a :href="item.address">
				<img src="wangyeico/wangye.png" class="logo">
				<div class="name">{{item.name}}</div>
			</a>
			<i class="iconfont icon-guanbi1" @click.stop="deleteCy(index)"></i>
		</div>
		<div class="cy-web" @click="isShow = true" v-if="cywebs.length < 6">
			<img :src="'wangyeico/tianjia.png'" class="logo">
		</div>
		<div class="add-cy-contain" v-if="isShow">
			<header>
				<span>添加常用网页</span>
				<i class="iconfont icon-guanbi" @click="closeAdd"></i>
			</header>
			<article>
				<input type="text" placeholder="请输入网页名字" v-model="addForm.name">
				<input type="text" placeholder="请输入网页地址" v-model="addForm.address">
			</article>
			<footer>
				<button @click="submitCy">确定</button>
			</footer>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'cywangzhi',
		data() {
			return {
				isShow: false,
				addForm: {
					name: '',
					address: ''
				},
				localStorage: window.localStorage,
				cywebs: new Array()
			}
		},
		created() {
			if (this.localStorage.getItem("cywebs")) {
				this.cywebs = JSON.parse(this.localStorage.getItem("cywebs"))
			}
		},
		methods: {
			closeAdd() {
				this.isShow = false
				this.addForm.name = this.addForm.address = ''
			},
			submitCy() {
				if (this.addForm.name != '' && this.addForm.address != '') {
					this.cywebs.push(this.addForm)
					this.isShow = false
					this.localStorage.setItem("cywebs",JSON.stringify(this.cywebs))
				} else {
					alert("添加失败")
				}
				this.addForm.name = this.addForm.address = ''
				this.cywebs = JSON.parse(this.localStorage.getItem("cywebs"))
			},
			deleteCy(index) {
				this.cywebs.splice(index, 1)
				this.localStorage.setItem("cywebs",JSON.stringify(this.cywebs))
				this.cywebs = JSON.parse(this.localStorage.getItem("cywebs"))
			}
		}
	}
</script>

<style scoped="scoped">
	.cy-contain{
		width: 50vw;
		position: absolute;
		bottom: 20vh;
		display: flex;
	}
	.cy-web{
		width: 7rem;
		height: 7rem;
		background: rgba(100,100,100,0.6);
		border-radius: 1em;
		cursor: pointer;
		transition: all 1s;
		margin-right: 2em;
		display: flex;
		justify-content: center;
		align-items: center;
		user-select: unset;
		position: relative;
	}
	.cy-web > a{
		width: 100%;
		height: 100%;
		color: rgba(233,233,233,0.6);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		text-decoration: none;
	}
	.cy-web:hover{
		background: rgba(0,168,255,0.6);
		transition: all 0.5s;
	}
	.cy-web:active{
		background-color: rgba(0,0,0,.7);
		transition: all 0.5s;
	}
	.cy-web > a > .logo{
		width: 3em;
		height: 3em;
	}
	.cy-web > a > .name{
		width: 80%;
		margin-top: 0.5em;
		font-size: 0.9em;
		box-sizing: border-box;
		text-align: center;
		overflow: hidden;
	}
	.cy-web > i {
		position: absolute;
		top: 0;
		right: 0.2em;
		font-size: 1.4em;
		color: rgba(233,233,233,0.6);
		opacity: 0;
		transition: all 0.5s;
	}
	.cy-web > i:hover{
		color: #333333;
		opacity: 1;
		transition: all 0.5s; 
	}
	
	.add-cy-contain{
		width: 20vw;
		height: 30vh;
		background-color: rgba(255,255,255,1);
		position: absolute;
		right: 0%;
		top: 50%;
		transform: translate(0%,-50%);
		border-radius: 1vw;
		padding: 1em;
		box-sizing: border-box;
		animation: fade 1s;
	}
	.add-cy-contain > header{
		width: 100%;
		height: 10%;
		font-size: 1.1em;
		display: flex;
		justify-content: space-between;
		align-items: center;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
	}
	
	.add-cy-contain > header > i{
		font-size: 1.1em;
		cursor: pointer;
		color: #a3a3a3;
		transition: all 0.5s;
	}
	.add-cy-contain > header > i:hover{
		color: #333;
		transition: all 0.5s;
	}
	.add-cy-contain > article{
		width: 100%;
		height: 70%;
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
	}
	.add-cy-contain > article > input{
		width: 80%;
		height: 2em;
		border-radius: 0.5em;
		border: 1px solid #dcdfe6;
		padding: 0 1em;
		outline: none;
	}
	.add-cy-contain > footer{
		width: 100%;
		height: 20%;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.add-cy-contain > footer > button{
		height: 100%;
		width: 25%;
		outline: none;
		border: none;
		background-color: #409eff;
		color: #fff;
		border-radius: 2em;
		cursor: pointer;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
		transition: all 0.5s;
	}
	.add-cy-contain > footer > button:hover{
		background-color: #66b1ff;
		transition: all 0.5s;
	}
	.add-cy-contain > footer > button:active{
		background-color: #3a8ee6;
		transition: all 0.5s;
	}
	@keyframes fade{
		from{opacity: 0;}
		to{opacity: 1;}
	}
</style>
