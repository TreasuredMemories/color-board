<template>
	<view>
		<view class="round_picker">
			<canvas id="canvas" canvas-id="boardcanvas" ref="canvas" style="width: 100%;
				height: 100%;" @touchmove="drag">
			</canvas>
		</view>
		<view class="slider">
			<slider activeColor="transparent" backgroundColor="transparent" class="ribbon" max="360"></slider>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'round-board',
		data() {
			return {
				x: 0,
				y: 0,
				centerX: 0,
				centerY: 0,
			};
		},
		mounted() {
			this.boardInit()
		},
		methods: {
			boardInit() {
				let ctx = uni.createCanvasContext('boardcanvas', this)
				console.log(ctx)
				var view = uni.createSelectorQuery().in(this).select("#canvas");
				view.boundingClientRect(data => {
					var width = data.width
					var height = data.height
					console.log("data")
					console.log(width, height)
					const centerX = width / 2;
					const centerY = height / 2;
					this.x = centerX
					this.y = centerY
					this.centerX = centerX
					this.centerY = centerY
					const radius = Math.min(centerX, centerY) - 10;
					// 绘制圆形调色板
					for (let angle = 0; angle < 360; angle++) {
						const startAngle = angle * Math.PI / 180;
						const endAngle = (angle + 1) * Math.PI / 180;
						const gradient = ctx.createCircularGradient(centerX, centerY, radius);
						gradient.addColorStop(0, this.hslTohex(angle, 1, 0.5));
						gradient.addColorStop(1, this.hslTohex(angle + 1, 1, 0.5));
						ctx.setFillStyle(gradient)
						ctx.beginPath()
						ctx.moveTo(centerX, centerY);
						ctx.arc(centerX, centerY, radius, startAngle, endAngle);
						ctx.closePath()
						ctx.fill()
					}
					ctx.draw()
				}).exec()

			},
			drag(event) {
				var view = uni.createSelectorQuery().in(this).select("#canvas");
				view.boundingClientRect(rect => {
					var width = rect.width
					var height = rect.height
					const centerX = width / 2;
					const centerY = height / 2;
					const radius = Math.min(centerX, centerY) - 10;
					const x = event.touches[0].x - this.centerX;
					const y = event.touches[0].y - this.centerY;
					const distance = Math.sqrt(x * x + y * y);
					var hue = Math.atan2(y, x) * 180 / Math.PI;
					if (hue < 0) {
						hue = 360 + hue
					}
					const saturation = distance / radius;
					const lightness = 0.5;
					const color = `hsl(${hue}, ${saturation * 100}%, ${lightness * 100}%)`;
					this.$emit('change', this.hslTohex(hue, saturation, lightness));
				}).exec()
			},
			/**
			 *
			 * @param {Number} H 色相 [0,360]
			 * @param {Number} S 饱和度 [0,1]
			 * @param {Number} L 亮度 [0,1]
			 */
			hslTohex(H, S, L) {
				const C = (1 - Math.abs(2 * L - 1)) * S
				const X = C * (1 - Math.abs(((H / 60) % 2) - 1))
				const m = L - C / 2
				const vRGB = []
				if (H >= 0 && H < 60) {
					vRGB.push(C, X, 0)
				} else if (H >= 60 && H < 120) {
					vRGB.push(X, C, 0)
				} else if (H >= 120 && H < 180) {
					vRGB.push(0, C, X)
				} else if (H >= 180 && H < 240) {
					vRGB.push(0, X, C)
				} else if (H >= 240 && H < 300) {
					vRGB.push(X, 0, C)
				} else if (H >= 300 && H <= 360) {
					vRGB.push(C, 0, X)
				}
				const [vR, vG, vB] = vRGB
				const R = 255 * (vR + m)
				const G = 255 * (vG + m)
				const B = 255 * (vB + m)
				// return `rgb(${R},${G},${B})`
				var hex = "#" + ((1 << 24) + (Math.round(R) << 16) + (Math.round(G) << 8) + Math.round(B)).toString(16)
					.slice(
						1);
				return hex

			}
		}
	}
</script>

<style>
	.round_picker {
		width: 90vw;
		height: 90vw;
		border-radius: 50%;
		overflow: hidden;
		position: relative;
		margin: 3vh auto;
	}

	#canvas {
		position: absolute;
		width: 100%;
		height: 100%;
	}

	.ring {
		background: radial-gradient(ellipse at center, rgb(247 235 234 / 5%) 42%, rgb(235 221 221 / 50%) 45%, rgb(237 230 230 / 50%) 50%, rgb(219 204 204 / 5%) 100%);
		border-radius: 50%;
		width: 32px;
		height: 32px;
		/* margin-top: -200px; */
		/* margin-left: -200px; */
		/* top: 14%; */
		/* left: 14%; */
		z-index: 3;
	}


	.ribbon {
		background: -webkit-linear-gradient(left, rgba(0, 0, 0, 1), rgba(255, 255, 255, 1));
		width: 90%;
		margin: 10rpx auto;
		border-radius: 5px;
	}
</style>
