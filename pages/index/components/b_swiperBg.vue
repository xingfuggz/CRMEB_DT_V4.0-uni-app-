<template>
	<view class="swiperBg" :class="{borderShow:isBorader}">
		<block>
			<view class="swiper square" :class="{borderShow:isBorader}" v-if="imgUrls.length">
				<swiper indicator-dots="true" :autoplay="true" :circular="circular" :interval="interval" :duration="duration"
				 indicator-color="rgba(255,255,255,0.6)" indicator-active-color="#fff">
					<block v-for="(item,index) in imgUrls" :key="index">
						<swiper-item>
							<view @click="goDetail(item)" class='slide-navigator acea-row row-between-wrapper'>
								<image :src="item.img" class="slide-image aa"></image>
							</view>
						</swiper-item>
					</block>
				</swiper>
			</view>
		</block>
	</view>
</template>

<script>
	let app = getApp();
	import {
		goPage
	} from '@/libs/order.js'
	export default {
		name: 'b_swiperBg',
		props: {
			dataConfig: {
				type: Object,
				default: () => {}
			},
			activeName: {
				type: String,
				default: ''
			}
		},
		watch: {
			activeName: {
				handler(nVal, oVal) {
					if (nVal == this.name && app.globalData.isIframe) {
						this.isBorader = true
					} else {
						this.isBorader = false
					}
				},
				deep: true
			}
		},
		data() {
			return {
				indicatorDots: false,
				circular: true,
				autoplay: true,
				interval: 3000,
				duration: 500,
				imgUrls: this.dataConfig.imgList.list, //图片轮播数据
				isBorader: false,
				name: this.$options.name
			};
		},
		created() {
			//this.imgUrls = []
		},
		mounted() {
			let that = this;
		},
		methods: {
			goDetail(url) {
				goPage().then(res => {
					let urls = url.info[0].value
					if(urls){
						if (urls.indexOf("http") != -1) {
							// #ifdef H5
							location.href = urls
							// #endif
						} else {
							if (['/pages/goods_cate/goods_cate', '/pages/order_addcart/order_addcart', '/pages/user/index'].indexOf(urls) ==
								-1) {
								uni.navigateTo({
									url: urls
								})
							} else {
								uni.switchTab({
									url: urls
								})
							}
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.swiperBg {
		position: relative;
		/* #ifdef MP */
		margin-top: 127rpx;

		/* #endif */
		.colorBg {
			position: absolute;
			left: 0;
			top: 0;
			height: 130rpx;
			width: 100%;
		}

		.swiper {
			z-index: 20;
			position: relative;
			// margin: -130rpx auto 0 auto;
			overflow: hidden;
			// image {
			//   width: 100%;
			//   height: auto;
			//   display: block;
			// }

			/* 设置圆角 */
			&.fillet {
				border-radius: 10rpx;

				image {
					border-radius: 10rpx;
				}
			}

			swiper,
			.swiper-item,
			image {
				width: 100%;
				height: 375rpx;
				display: block;
			}

			// 圆形指示点
			&.circular {
				/deep/.uni-swiper-dot {
					width: 10rpx !important;
					height: 10rpx !important;
					background: rgba(0, 0, 0, .4) !important
				}

				/deep/.uni-swiper-dot-active {
					background: #fff !important
				}
			}

			// 方形指示点
			&.square {
				/deep/.uni-swiper-dot {
					width: 20rpx !important;
					height: 5rpx !important;
					border-radius: 3rpx;
					background: rgba(0, 0, 0, .4) !important
				}

				/deep/.uni-swiper-dot-active {
					background: #fff !important
				}
			}
		}
	}

	.item-img image {
		display: block;
		width: 100%;
	}
</style>
