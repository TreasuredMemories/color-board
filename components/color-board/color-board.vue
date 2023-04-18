<template>
	<view class="page">
		<!-- header -->
		<view class="header">
			<view :style="[{padding:StatusBar + 'px'}]">
				<view class="flex">
					<view class="dot" style="background-color: red;"></view>
					<view class="dot" style="background-color: yellow;"></view>
					<view class="dot" style="background-color: green;"></view>
				</view>
			</view>
		</view>
		<!-- copy area -->
		<view class="copy_area flex">
			<view class="copy_box flex">
				<view class="copy_button flex-sub">
					<view class="cuIcon-copy solid-right"></view>
				</view>
				<view class="color_value flex-treble">{{chooseColor}}</view>
				<view class="color_area flex-sub">
					<view class="color_back" :style="{backgroundColor: chooseColor}"></view>
				</view>
			</view>
		</view>
		<!-- mode -->
		<view class="mode_container">
			<view class="mode_box">
				<scroll-view scroll-x class="nav mode_scroll">
					<view class="flex text-center padding-xs">
						<view class="cu-item mode_item flex-sub bg-black" :class="index==TabCur?'light':''"
							v-for="(item,index) in modeList" :key="index" @tap="tabSelect" :data-id="index">
							{{item.name}}
						</view>
					</view>
				</scroll-view>
			</view>
		</view>
		<!-- container -->
		<view>
			<!-- board -->
			<view>
				<roundboard @change="pickColor"></roundboard>
			</view>
			<!-- slider -->
			<view></view>
		</view>

	</view>
</template>

<script>
	import roundboard from "./mode/round-board.vue"
	export default {
		name: 'color-board',
		components: {
			roundboard
		},
		data() {
			return {
				StatusBar: this.StatusBar,
				CustomBar: this.CustomBar,
				TabCur: 0,
				scrollLeft: 0,
				chooseColor: '#fff',
				modeList: [{
						name: "色盘",
					},
					{
						name: "经典"
					},
					{
						name: "值"
					},
					{
						name: "CMYK"
					}
				]
			};
		},
		methods: {
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			},
			pickColor(color) {
				this.chooseColor = color
			}
		}
	}
</script>

<style>


	/* color */
	.bg-black {
		background-color: black;
		color: #555353;
	}

	.bg-black.light {
		background-color: #28282a;
		color: #a5a5a5;
	}

	/* components */

	.page {
		height: 100vh;
		background-color: #333333;
	}

	.header {
		background-color: #393b3f;
	}

	.dot {
		cursor: pointer;
		margin: 0 5px;
		width: 15px;
		height: 15px;
		border-radius: 50%;
		box-sizing: border-box;
		transition: all .2s;
	}

	.copy_area {
		width: 100%;
		line-height: 90upx;

	}

	.copy_box {
		margin: 30upx 30upx 5upx 30upx;
		width: 100%;
		background-color: #464646;
		color: #a5a5a5;
		border-radius: 5px;
	}

	.copy_button {
		text-align: center;
	}

	.color_area {
		padding: 10upx;
	}

	.color_value {
		padding: 0upx 20upx;
	}

	.color_back {
		border-radius: 10upx;
		width: 100%;
		height: 100%;
	}

	.mode_container {
		padding: 30upx;
	}

	.mode_box {
		border-radius: 20px;
		background-color: black;

	}

	.mode_scroll {
		border-radius: 10px;
	}

	.mode_item {
		border-radius: 10px;
	}

</style>
