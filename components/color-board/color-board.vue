<template>
	<view class="page">
		<!-- header -->
		<view :style="[{height:CustomBar + 'px'}]">
			<view :style="[{padding:StatusBar + 'px'}]">

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
			<view></view>
			<!-- slider -->
			<view></view>
		</view>

	</view>
</template>

<script>
	export default {
		name: 'color-board',
		data() {
			return {
				StatusBar: this.StatusBar,
				CustomBar: this.CustomBar,
				TabCur: 0,
				scrollLeft: 0,
				modeList: [{
						name: "色盘",
					},
					{
						name: "经典"
					},
					{
						name: "值"
					}
				]
			};
		},
		methods: {
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			}
		}
	}
</script>

<style>
	.nav {
		white-space: nowrap;
	}


	.nav .cu-item {
		height: 90upx;
		display: inline-block;
		line-height: 90upx;
		margin: 0 10upx;
		padding: 0 20upx;
	}

	/* layout */
	.flex {
		display: flex;
	}

	.flex-sub {
		flex: 1;
	}

	.padding-xs {
		padding: 10upx;
	}

	.text-center {
		text-align: center;
	}

	.nav .cu-item {
		height: 90upx;
		display: inline-block;
		line-height: 90upx;
		margin: 0 10upx;
		padding: 0 20upx;
	}

	.nav .cu-item.cur {
		border-bottom: 4upx solid;
	}

	/* color */
	.bg-black {
		background-color: black;
		color: #555353;
	}

	.bg-black.light {
		background-color: #28282a;
		color: #a5a5a5;
	}

	.page {
		height: 100vh;
		background-color: #333333;
	}

	.mode_container{
		padding: 30upx;
		
	}

	.mode_box {
		border-radius: 20px;
		background-color: black;

	}

	.mode_scroll {
		border-radius: 20px;
	}

	.mode_item {
		border-radius: 20px;
	}
</style>
