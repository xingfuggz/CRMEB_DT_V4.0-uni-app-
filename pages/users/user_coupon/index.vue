<template>
	<view>
		<view class='coupon-list' v-if="couponsList.length">
			<view class='item acea-row row-center-wrapper' v-for='(item,index) in couponsList' :key="index">
				<view class='money' :class='item._type == 0 ? "moneyGray" : ""'>
					<view>￥<text class='num'>{{item.coupon_price}}</text></view>
					<view class="pic-num">满{{ item.use_min_price }}元可用</view>
				</view>
				<view class='text'>
					<view class='condition line1'>
						<view class="line-title" :class="item._type === 0 ? 'bg-color-huic' : 'bg-color-check'" v-if="item.applicable_type === 0">通用劵</view>
						<view class="line-title" :class="item._type === 0 ? 'bg-color-huic' : 'bg-color-check'" v-else-if="item.applicable_type === 1">品类券</view>
						<view class="line-title" :class="item._type === 0 ? 'bg-color-huic' : 'bg-color-check'" v-else>商品券</view>
						<view>{{item.coupon_title}}</view>
					</view>
					<view class='data acea-row row-between-wrapper'>
						<view>{{item._add_time}}-{{item._end_time}}</view>
						<view class='bnt gray' v-if="item._type==0">{{item._msg}}</view>
						<view class='bnt bg-color' v-else>{{item._msg}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class='noCommodity' v-if="!couponsList.length && loading==true">
			<view class='pictrue'>
				<image src='../../../static/images/noCoupon.png'></image>
			</view>
		</view>
		<!-- #ifdef MP -->
		<authorize @onLoadFun="onLoadFun" :isAuto="isAuto" :isShowAuth="isShowAuth" @authColse="authColse"></authorize>
		<!-- #endif -->
		<home></home>
	</view>
</template>

<script>
	import {
		getUserCoupons
	} from '@/api/api.js';
	import {
		toLogin
	} from '@/libs/login.js';
	import {
		mapGetters
	} from "vuex";
	// #ifdef MP
	import authorize from '@/components/Authorize';
	// #endif
	import home from '@/components/home';
	export default {
		components: {
			// #ifdef MP
			authorize,
			// #endif
			home
		},
		data() {
			return {
				couponsList: [],
				loading: false,
				isAuto: false, //没有授权的不会自动授权
				isShowAuth: false //是否隐藏授权
			};
		},
		computed: mapGetters(['isLogin']),
		onLoad() {
			if (this.isLogin) {
				this.getUseCoupons();
			} else {
				// #ifdef H5 || APP-PLUS
				toLogin();
				// #endif 
				// #ifdef MP
				this.isAuto = true;
				this.$set(this, 'isShowAuth', true)
				// #endif
			}
		},
		methods: {
			/**
			 * 授权回调
			 */
			onLoadFun: function() {
				this.getUseCoupons();
			},
			// 授权关闭
			authColse: function(e) {
				this.isShowAuth = e
			},
			/**
			 * 获取领取优惠券列表
			 */
			getUseCoupons: function() {
				let that = this;
				getUserCoupons(0).then(res => {
					that.loading = true;
					that.$set(that, 'couponsList', res.data);
				})
			}
		}
	}
</script>

<style>
	.money {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.pic-num {
		color: #ffffff;
		font-size: 0.24rem;
	}
	.coupon-list .item .text .condition{
		display: flex;
		align-items: center;
	}
	.condition .line-title {
		width: 90rpx;
		height: 40rpx !important;
		line-height: 1.5 !important;
		padding: 0 10rpx;
		-webkit-box-sizing: border-box;
		box-sizing: border-box;
		background: rgba(255, 247, 247, 1);
		border: 1px solid rgba(232, 51, 35, 1);
		opacity: 1;
		border-radius: 22rpx;
		font-size: 20rpx !important;
		color: #e83323;
		margin-right: 12rpx;
	}
</style>
