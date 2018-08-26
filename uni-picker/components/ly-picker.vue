<template>
	<view v-if="show">
		<view class="ly-picker">
			<view class="ly-picker-header">
				<button size="mini" @click="cancel()">取消</button>
				<button type="primary" size="mini" style="margin-right: 20px;float: right;" @click="sure">确定</button>
			</view>
			<view class="ly-picker-body">
				<scroll-view @touchend="touchend" scroll-y @scroll="scroll" :scroll-top="scrollTop" :style="'width:'+scrollWidth+'px'" v-if="type>0"
				    scroll-with-animation>
					<view style="height: 200px;"></view>
					<view class="ly-picker-active"></view>
					<view v-for="(item,index) in list" :key="index" class="ly-picker-body-item">
						<view>{{item[name]}}</view>
					</view>
					<view style="height: 200px;"></view>
				</scroll-view>
				<scroll-view @touchend="touchend2" scroll-y @scroll="scrollSecond" :scroll-top="secondScrollTop" :style="'width:'+scrollWidth+'px'"
				    v-if="type>1">
					<view style="height: 200px;"></view>
					<view v-for="(item,index) in secondList" :key="index" class="ly-picker-body-item">
						<view>{{item[name]}}</view>
					</view>
					<view style="height: 200px ;"></view>
				</scroll-view>
				<scroll-view @touchend="touchend3" scroll-y @scroll="scrollThree" :scroll-top="threeScrollTop" :style="'width:'+scrollWidth+'px'"
				    v-if="type>2">
					<view style="height: 200px;"></view>
					<view v-for="(item,index) in threeList" :key="index" class="ly-picker-body-item">
						<view>{{item[name]}}</view>
					</view>
					<view style="height: 200px;"></view>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	var scrollTopTmp, scrollTopTmp2, scrollTopTmp3
	var scrollHeight, scrollHeight2, scrollHeight3
	export default {
		name: "page-head",
		data: () => {
			return {
				height:0,
				scrollTop: 0,
				selectIndex: 0,

				secondScrollTop: 0,
				secondSelectIndex: 0,
				secondList: [],

				threeScrollTop: 0,
				threeSelectIndex: 0,
				threeList: [],
				scrollWidth: 0,
			}
		},
		props: {
			show: {
				type: Boolean,
				default: false
			},
			list: {
				type: Array,
				default: () => {
					return null
				}
			},
			type: {
				type: Number,
				default: 1
			},
			name: {
				type: String,
				default: "name"
			},
			children: {
				type: String,
				default: "children"
			},
			callBackFun: {}
		},
		methods: {
			touchend() {
				uni.showLoading({
					mask: true,
					title: "请稍后",
				})
				let itemHeight = Math.round((scrollHeight - 200) / this.list.length)
				let activeIndex = Math.round(scrollTopTmp / itemHeight)
				this.selectIndex = activeIndex == this.list.length ? this.list.length - 1 : activeIndex
				this.scrollTop = Math.round(itemHeight * this.selectIndex )
				if (this.type > 1 && this.list.length > 0 && this.list[this.selectIndex].hasOwnProperty(this.children)) {
					this.secondList = this.list[this.selectIndex][this.children]
					this.secondScrollTop = 0
					this.secondSelectIndex = 0
				}
				if (this.type > 2 && this.secondList.length > 0 && this.secondList[this.secondSelectIndex].hasOwnProperty(this.children)) {
					this.threeSelectIndex = 0
					this.threeScrollTop = 0
					this.threeList = this.secondList[this.secondSelectIndex][this.children]
				}
				setTimeout(() => {
					uni.hideLoading()
				}, 500)
			},
			touchend2() {
				uni.showLoading({
					mask: true,
					title: "请稍后",
				})
				let itemHeight = Math.round((scrollHeight2 - 200) / this.secondList.length)
				let activeIndex = Math.round(scrollTopTmp2 / itemHeight)
				this.secondSelectIndex = activeIndex == this.secondList.length ? this.secondList.length - 1 : activeIndex
				this.secondScrollTop = Math.round(itemHeight * this.secondSelectIndex)
				if (this.type > 2 && this.secondList.length > 0 && this.secondList[this.secondSelectIndex].hasOwnProperty(this.children)) {
					this.threeList = this.secondList[this.secondSelectIndex][this.children]
					this.threeScrollTop = 0
					this.threeSelectIndex = 0
				}
				setTimeout(() => {
					uni.hideLoading()
				}, 500)
			},
			touchend3() {
				uni.showLoading({
					mask: true,
					title: "请稍后",
				})
				let itemHeight = Math.round((scrollHeight3 - 200) / this.threeList.length)
				let activeIndex = Math.round(scrollTopTmp3 / itemHeight)
				this.threeSelectIndex = activeIndex == this.threeList.length ? this.threeList.length - 1 : activeIndex
				this.threeScrollTop = Math.round(itemHeight * this.threeSelectIndex)
				setTimeout(() => {
					uni.hideLoading()
				}, 500)
			},
			cancel() {
				this.show = false
				this.$emit('update:show', this.show)
			},
			scroll(e) {
				scrollTopTmp = e.detail.scrollTop
				scrollHeight = e.detail.scrollHeight
			},
			scrollSecond(e) {
				scrollTopTmp2 = e.detail.scrollTop
				scrollHeight2 = e.detail.scrollHeight
			},
			scrollThree(e) {
				scrollTopTmp3 = e.detail.scrollTop
				scrollHeight3 = e.detail.scrollHeight
			},
			sure() {
				this.show = false
				this.$emit('update:show', this.show)
				console.log(this.scrollTop)
				if (this.type == 1) {
					this.callBackFun([this.list[this.selectIndex]])
				} else if (this.type == 2) {
					this.callBackFun([this.list[this.selectIndex], this.secondList[this.secondSelectIndex]])
				} else if (this.type == 3) {
					this.callBackFun([this.list[this.selectIndex], this.secondList[this.secondSelectIndex], this.threeList[this.threeSelectIndex]])
				}
			}
		},
		mounted: function () {
			uni.getSystemInfo({
				success: res => {
					let width = res.screenWidth
					this.scrollWidth = width / this.type
				}
			})
		},
		watch: {
			'show': function (newValue, oldValue) {
				if (newValue) {
					this.selectIndex = 0
					this.scrollTop = 0
					this.secondSelectIndex = 0
					this.secondScrollTop = 0
					this.threeSelectIndex = 0
					this.threeScrollTop = 0
					if (this.type > 1 && this.list.length > 0 && this.list[this.selectIndex].hasOwnProperty(this.children)) {
						this.secondList = this.list[this.selectIndex][this.children]
					} else {
						this.secondList = []
					}
					if (this.type > 2 && this.secondList.length > 0 && this.secondList[this.secondSelectIndex].hasOwnProperty(this.children)) {
						this.threeList = this.secondList[this.secondSelectIndex][this.children]
					} else {
						this.threeList = []
					}
				}
			}
		},
	}
</script>

<style>
	::-webkit-scrollbar {
		width: 0;
		height: 0;
		color: transparent;
	}

	.ly-picker {
		z-index: 999;
		width: 100%;
		padding-right: 100px;
		position: fixed;
		bottom: 0;
		background-color: #EEEEEE;
		display: block;
	}

	.ly-picker-header {
		width: 100%;
		height: 70px;
		padding: 10px;
	}

	.ly-picker scroll-view {
		height: 500px;
		text-align: center;
	}

	.ly-picker-body {
		display: flex;
	}

	.ly-picker-body-item {
		height: 100px;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.ly-picker-active {
		position: fixed;
		z-index: 1000;
		border-top: solid 4px #00AA00;
		border-bottom: solid 4px #00AA00;
		width: 100%;
		height: 100px;
	}
</style>
