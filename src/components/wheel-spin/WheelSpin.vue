<template>
	<section class="vue-winwheel">
		<div class="mobile-container">
			<h1>{{pageTitle}}</h1>
			<div class="wheel-wrapper">
				<div class="canvas-wrapper">
					<canvas id="canvas" :width="getWheelSize()" :height="getWheelSize()">
							<p style="{color: white}" align="center">
								Sorry, your browser doesn't support canvas. Please try Google Chrome.
							</p>
						</canvas>
				</div>
				<div class="button-wrapper">
					<a class="btn btn-play" :style="getBtnColor()" @click.prevent="startSpin()" v-if="!loadingPrize && !wheelSpinning">{{getBtnText()}}
						</a>
				</div>
			</div>
		</div>
		<prizeModal v-if="showModal" @close="resetWheel()" :prizeName="modalPrize.text" :prizeDesc="modalPrize.desc">
		</prizeModal>
	</section>
</template>


<script>
	import * as Winwheel from './Winwheel'
	import prizeModal from './prizeModal'
	export default {
        name: 'WheelSpin',
		components: {
			prizeModal
		},
		props: {
			btnColor: String,
			btnText: String,
			pageTitle: String,
			wheelSize: null,
			segments: Array,
			duration: Number,
			spin: Number,
			textOrientation: String,
			textAlignment: String,
			textMargin: Number,
		},
		data() {
			return {
				loadingPrize: false,
				theWheel: null,
				modalPrize: {},
				showModal: false,
				wheelPower: 1,
				wheelSpinning: false,
				WinWheelOptions: {
					drawMode: 'image',
					drawText: false,
					wheelmage: null,
					textFontSize: 16,
					outterRadius: 650,
					innerRadius: 25,
					lineWidth: 8,
					animation: {
						type: 'spinOngoing',
						duration: 0.5
					}
				}
			}
		},
		methods: {
			getWheelSize() {
				if (this.wheelSize) {
					return this.wheelSize
				} else {
					return 400
				}
			},
			hidePrize() {
				this.modalPrize = {}
				this.showModal = true
			},
			calcSegSize() {
				this.segments.forEach(function(seg) {
					if (seg.size) {
						seg.size = Winwheel.winwheelPercentToDegrees(seg.size)
					}
				})
			},
			startSpin() {
				if (this.wheelSpinning === false) {
					// this.theWheel.animation.stopAngle = this.theWheel.getRandomForSegment(2)
					this.theWheel.startAnimation()
					this.wheelSpinning = true
				}
			},
			resetWheel() {
				this.showModal = false
				this.theWheel = new Winwheel.Winwheel({
					...this.WinWheelOptions,
					textOrientation: this.textOrientation,
					textAlignment: this.textAlignment,
					textMargin: this.textMargin,
					numSegments: this.segments.length,
					segments: this.segments,
					animation: {
						type: 'spinToStop',
						easing: 'Power4.easeInOut',
						duration: this.duration,
						spins: this.spin,
						callbackFinished: this.onFinishSpin
					}
				})

				// Set background image
				let loadedImg = new Image()
				let _self = this
				// Create callback to execute once the image has finished loading.
				loadedImg.onload = function()
				{
					_self.theWheel.wheelImage = loadedImg    // Make wheelImage equal the loaded image object.
					_self.theWheel.draw()                    // Also call draw function to render the wheel.
				}
				// Set the image source, once complete this will trigger the onLoad callback (above).
				loadedImg.src = "wheel.png"

				if (this.wheelSpinning) {
					this.theWheel.stopAnimation(false) // Stop the animation, false as param so does not call callback function.
				}
				this.theWheel.rotationAngle = 0 // Re-set the wheel angle to 0 degrees.
				this.theWheel.draw() // Call draw to render changes to the wheel.
				this.wheelSpinning = false // Reset to false to power buttons and spin can be clicked again.
			},
			initSpin() {
				this.loadingPrize = true
				this.resetWheel()
				this.loadingPrize = false
			},
			onFinishSpin() {
				let prize = this.theWheel.getIndicatedSegment()
				this.modalPrize = {
					text: prize.text,
					desc: prize.desc
				}
				this.showModal = true
				this.wheelSpinning = false
			},
			getBtnColor() {
				let style = 'cursor:pointer; background:'
				if (this.btnColor) {
					return `${style}${this.btnColor};`
				} else {
					return `${style}#c4376f;`
				}
			},
			getBtnText() {
				if (this.btnText) {
					return this.btnText
				} else {
					return 'SPIN!'
				}
			}
		},
		computed: {},
		updated() {},
		mounted() {
			this.calcSegSize()
			this.initSpin()
		},
	}
</script>

<style scoped>
	.vue-winwheel {
		text-align: center;
		background-image: url('/static/img/obstacle-run/bg-spinner-mobile.svg');
		background-size: cover;
		background-position: center bottom;
		background-repeat: no-repeat;
	}
	.vue-winwheel h1 {
		color: #b32656;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		font-size: 36px;
		line-height: 90px;
		letter-spacing: 4px;
		margin: 0;
	}
	.vue-winwheel h2 {
		margin: 0;
	}
	.vue-winwheel #modalSpinwheel.custom-modal .content-wrapper .content {
		width: calc(100vw - 30px);
		padding-top: 52px;
	}
	.vue-winwheel #modalSpinwheel.custom-modal .content-wrapper .content h2 {
		text-transform: uppercase;
		color: #b32656;
		margin-bottom: 16px;
		margin-top: 0;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		font-size: 18px;
		letter-spacing: 1.1px;
		margin: 0;
	}
	.vue-winwheel #modalSpinwheel.custom-modal .content-wrapper .content p {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		font-size: 14px;
		color: black;
		text-align: center;
		line-height: 25px;
	}
	.vue-winwheel #modalSpinwheel.custom-modal .content-wrapper .content p strong {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
	}
	.vue-winwheel #modalSpinwheel.custom-modal .content-wrapper .content .modal-dismiss {
		top: 12px;
		right: 12px;
	}
	.vue-winwheel #modalSpinwheel.custom-modal .content-wrapper .content .modal-dismiss i.icon_close {
		font-size: 30px;
		color: #da2a52;
	}
	.vue-winwheel canvas#canvas {
		position: relative;
	}
	.vue-winwheel .canvas-wrapper {
		position: relative;
	}
	.vue-winwheel .canvas-wrapper:after {
		content: '';
		display: block;
		width: 42px;
		background: #c4376f;
		height: 42px;
		position: absolute;
		left: calc(50% - 25px);
		margin: auto;
		border-radius: 100%;
		top: calc(50% - 29px);
		border: 5px solid white;
		box-sizing: content-box;
	}
	.vue-winwheel .canvas-wrapper:before {
		content: '';
		display: block;
		background: #0f0f0f;
		position: absolute;
		left: 0;
		right: 0;
		margin: 0 auto;
		border-radius: 100%;
		top: 0;
	}
	.vue-winwheel .wheel-wrapper {
		position: relative;
	}
	.vue-winwheel .wheel-wrapper:before {
		content: '';
		width: 62px;
		height: 47px;
		position: absolute;
		top: -10px;
		left: calc(50% - 31px);
		right: 0;
		display: block;
		z-index: 99999;
		background-image: url('./spinner-marker.svg');
		background-repeat: no-repeat;
		background-size: contain;
		background-position: center;
	}
	.vue-winwheel .wheel-wrapper .button-wrapper {
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 231px;
		height: 118px;
	}
	.vue-winwheel .wheel-wrapper .btn.btn-play {
		padding: 0 58px !important;
		height: 40px;
		line-height: 40px;
		color: white;
		font-weight: bold;
		text-decoration: none;
		border-radius: 2px;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		letter-spacing: 2px;
	}
</style>