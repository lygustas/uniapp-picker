<template>
	<view>
		<view class="padding"></view>
		<view class="content">
			<view class="content">
				<button type="primary" @click="oneShow=true">一级选择</button>您选择的是:{{one}}
				<ly-picker :list="list" :show.sync="oneShow" :callBackFun="oneCallFun" :type="1" name="text"></ly-picker>
			</view>
			<view class="content">
				<button type="primary" @click="twoShow=true">二级选择</button>您选择的是:{{two}}
				<ly-picker :list="list" :show.sync="twoShow" :callBackFun="twoCallFun" :type="2" name="text" children="children"></ly-picker>
			</view>
			<view class="content">
				<button type="primary" @click="threeShow=true">三级选择</button>您选择的是:{{three}}
				<ly-picker :list="list" :show.sync="threeShow" :callBackFun="threeCallFun" :type="3" name="text" children="children"></ly-picker>
			</view>
		</view>
	</view>
</template>

<script>
	var cityData = require('../../common/cityData.js').threeCityData;
	import lyPicker from "../../components/ly-picker.vue"
	export default {
		components: {
			lyPicker
		},
		data: {
			one: "",
			two: "",
			three: "",
			oneShow: false,
			twoShow: false,
			threeShow: false,
			// list:[],
			list: ["A", "B", "C", "D", "E", "F", "G", "H", "J", "K", "L", "M", "N", "O", "P", "Q", "W", "V", "U", "X", "Y", "Z"]
		},
		methods: {
			oneCallFun(res) {
				this.one = res[0].text;
			},
			twoCallFun(res) {
				this.two = res[0].text+","+res[1].text;
			},
			threeCallFun(res) {
				this.three = res[0].text+","+res[1].text+","+res[2].text;
			},
		},
		mounted: function () {
			this.list = cityData.data;
		}
	}
</script>

<style>
	.picker {
		width: 100%;
		position: fixed;
		bottom: 0;
		background-color: rgba(0, 0, 0, .1);
	}

	.picker-header {
		width: 100%;
		height: 70px;
		padding: 10px;
	}

	.picker scroll-view {
		height: 500px;
		text-align: center;
		line-height: 90px;
	}

	.picker-body-item {
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.picker-active {
		z-index: 999;
		border-top: solid 4px #00AA00;
		border-bottom: solid 4px #00AA00;
		width: 100%;
	}

	.padding {
		height: var(--status-bar-height);
		position: fixed;
		top: 0;
		background-color: #3388FF;
	}

	.content {
		padding-top: var(--status-bar-height);
	}
</style>
