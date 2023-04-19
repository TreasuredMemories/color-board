<template>
	<view>
		<view class="box">
			<slider @change="changeHue"  activeColor="transparent" backgroundColor="transparent" class="ribbon" max="360"
				:value="hsv.h" :block-color="colorRes" @touchend="onEnd"></slider>
			<movable-area class="target" @tap="areaTap" :style="{backgroundColor:hueColor}">
				<movable-view direction="all" @touchend="onEnd" @change="changeSV" :x="x" :y="y">
					<text class="cuIcon-colorlens text-xl text-white"></text>
				</movable-view>
			</movable-area>
		</view>
	</view>
</template>

<script>
	export default {
		name: "pickColor",
		props: {
			initColor: {
				type: String,
				default: 'rgb(255,0,0)'
			},
			maskClosable: {
				type: Boolean,
				default: true
			},
			mask: {
				type: Boolean,
				default: true
			},
			show: {
				type: Boolean,
				default: false
			},
		},
		watch: {
			modelShow: {
				handler(newName, oldName) {
					console.log("modelShow")
					console.log(newName)
					this.show = newName
				},
				deep: true
			},
			collectLists(news, olds) {
				this.collectList = news
			}
		},
		data() {
			return {
				hueColor: '',
				x: 1,
				y: 2,
				SV: {},
				hsv: {},
				colorRes: ''
			};
		},
		mounted() {
			this.hueColor = this.hsv2rgb(this.rgb2hsv(this.initColor).h, 100, 100)
			console.log(this.hueColor)
			var view = uni.createSelectorQuery().in(this).select(".target");
			view.boundingClientRect(data => {
				console.log("得到布局位置信息" + JSON.stringify(data));
				this.$nextTick(() => {
					this.SV = {
						W: data.width - 28,
						H: data.height - 28,
						Step: (data.width - 28) / 100
					}
					let {
						h,
						s,
						v
					} = this.rgb2hsv(this.initColor)
					this.hsv = {
							h,
							s,
							v
						},
						this.x = Math.round(s * this.SV.Step)
					this.y = Math.round((100 - v) * this.SV.Step)
				})
			}).exec();
		},
		onReady() {
			
		},
		methods: {
			hideCollectModal() {
				this.show = false;
			},
			changeSV(e) {
				let {
					x,
					y
				} = e.detail;
				x = Math.round(x / this.SV.Step);
				y = 100 - Math.round(y / this.SV.Step);
				this.hsv.s = x,
					this.hsv.v = y,
					this.colorRes = this.hsv2rgb(this.hsv.h, x, y)
			},
			onEnd() {
				this.$emit('change', this.colorRes);
			},
			changeHue(e) {
				let hue = e.detail.value;
				this.hsv.h = hue
				this.hueColor = this.hsv2rgb(hue, 100, 100)
				this.colorRes = this.hsv2rgb(hue, this.hsv.s, this.hsv.v)
			},
			hsv2rgb(h, s, v) {
				let hsv_h = (h / 360).toFixed(2);
				let hsv_s = (s / 100).toFixed(2);
				let hsv_v = (v / 100).toFixed(2);

				var i = Math.floor(hsv_h * 6);
				var f = hsv_h * 6 - i;
				var p = hsv_v * (1 - hsv_s);
				var q = hsv_v * (1 - f * hsv_s);
				var t = hsv_v * (1 - (1 - f) * hsv_s);

				var rgb_r = 0,
					rgb_g = 0,
					rgb_b = 0;
				switch (i % 6) {
					case 0:
						rgb_r = hsv_v;
						rgb_g = t;
						rgb_b = p;
						break;
					case 1:
						rgb_r = q;
						rgb_g = hsv_v;
						rgb_b = p;
						break;
					case 2:
						rgb_r = p;
						rgb_g = hsv_v;
						rgb_b = t;
						break;
					case 3:
						rgb_r = p;
						rgb_g = q;
						rgb_b = hsv_v;
						break;
					case 4:
						rgb_r = t;
						rgb_g = p;
						rgb_b = hsv_v;
						break;
					case 5:
						rgb_r = hsv_v, rgb_g = p, rgb_b = q;
						break;
				}

				return 'rgb(' + (Math.floor(rgb_r * 255) + "," + Math.floor(rgb_g * 255) + "," + Math.floor(rgb_b * 255)) +
					')';
			},
			rgb2hsv(color) {
				let rgb = color.split(',');
				let R = parseInt(rgb[0].split('(')[1]);
				let G = parseInt(rgb[1]);
				let B = parseInt(rgb[2].split(')')[0]);

				let hsv_red = R / 255,
					hsv_green = G / 255,
					hsv_blue = B / 255;
				let hsv_max = Math.max(hsv_red, hsv_green, hsv_blue),
					hsv_min = Math.min(hsv_red, hsv_green, hsv_blue);
				let hsv_h, hsv_s, hsv_v = hsv_max;

				let hsv_d = hsv_max - hsv_min;
				hsv_s = hsv_max == 0 ? 0 : hsv_d / hsv_max;

				if (hsv_max == hsv_min) hsv_h = 0;
				else {
					switch (hsv_max) {
						case hsv_red:
							hsv_h = (hsv_green - hsv_blue) / hsv_d + (hsv_green < hsv_blue ? 6 : 0);
							break;
						case hsv_green:
							hsv_h = (hsv_blue - hsv_red) / hsv_d + 2;
							break;
						case hsv_blue:
							hsv_h = (hsv_red - hsv_green) / hsv_d + 4;
							break;
					}
					hsv_h /= 6;
				}
				return {
					h: (hsv_h * 360).toFixed(),
					s: (hsv_s * 100).toFixed(),
					v: (hsv_v * 100).toFixed()
				}
			},
			areaTap(e) {
				var view = uni.createSelectorQuery().in(this).select(".target");
				view.boundingClientRect(data => {
					console.log("得到布局位置信息" + JSON.stringify(data));
					this.$nextTick(() => {
						this.x = e.detail.x - data.left - 14
						this.y = e.detail.y - data.top - 14
						this.changeSV({
							detail: {
								x: this.x,
								y: this.y
							}
						})
						this.onEnd()
					})
				}).exec();
			}
		}
	}
</script>

<style>
	.box {
		width: 90vw;
		height: 100vw;
		overflow: hidden;
		position: relative;
		margin: 3vh auto;
	}

	.target {
		height: 90vw;
		width: 90vw;
		margin: 0 auto;
		background: url(@/static/img/picker_mask.png) no-repeat center / cover;
		overflow: hidden;
	}

	.ribbon {
		background: -webkit-linear-gradient(left, #f00 0%, #ff0 17%, #0f0 33%, #0ff 50%, #00f 67%, #f0f 83%, #f00 100%);
		width: 90%;
		border-radius: 10px;
	}
</style>
