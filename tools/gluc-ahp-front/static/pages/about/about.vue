<template>
	<view class="page">
		<view class='base-padding row'>
			<view class='user'>
				<image :src='avatar_path'></image>
				<view class='username font-lv2'>{{info.app_name}}</view>
			</view>
			<view v-if="info.slogan" class='color-grey font-lv3 user-intro'>{{info.slogan}}</view>
		</view>

		<view class='base-padding row base-info font-lv2'>
			<view class='col-12 text-center'>
				<view v-if="info.app_version && info.app_version.length > 0">
					<text class='font-lv4 color-grey'>
						软件版本：{{info.app_version}}
					</text>
				</view>
				<view  v-if="info.json_revision_ver && info.json_revision_ver.length > 0">
					<text class='font-lv4 color-grey'>
						校对版本号：{{info.json_revision_ver}}
					</text>
				</view>
				<view v-if="info.json_revision_date && info.json_revision_date.length > 0">
					<text class='font-lv4 color-grey'>
						校对日期：{{info.json_revision_date}}
					</text>
				</view>				
				<view v-if="info.license && info.license.length > 0">
					<text class='font-lv4 color-grey'>
						license：{{info.license}}
					</text>
				</view>
			</view>
		</view>

		<view class='base-padding row base-info font-lv2'>
			<view class='col-12 text-center'>
				<view v-if="info.data_src_name && info.data_src_name.length > 0">
					<text class='font-lv4 color-grey'>
						本软件的主要数据来源：{{info.data_src_name}}{{info.data_src_ver}}
					</text>
				</view>
				<view v-if="info.data_src_date && info.data_src_date.length > 0">
					<text class='font-lv4 color-grey'>
						数据源日期：{{info.data_src_date}}
					</text>
				</view>
			</view>
		</view>

		<view v-if="info.other_desc && info.other_desc.length > 0" class='base-padding row base-info font-lv2'>
			<view class='col-12 text-center'>
				<view>
					<text class='font-lv4 color-grey'>
						{{info.other_desc}}
					</text>
				</view>
			</view>
		</view>
		

		<view v-if="info.contacts && info.contacts.length > 0" class='base-padding row base-info font-lv2'>
			<view class='col-12 text-center'>
				<view>
					<text class='font-lv4 color-grey'>
						{{info.contacts}}
					</text>
				</view>
			</view>
		</view>

		<view class='base-padding row base-info footer'>
			<view class='col-12 text-center'>
				<view>
					<text class='font-lv4 color-grey'>Copyright © {{now}} {{info.app_name}}</text>
				</view>
				<view>
					<text class='font-lv4 color-grey'>作者：{{info.author}}</text>
				</view>
				<view>
					<text class='font-lv4 color-grey'>校对：{{info.revisionor}}</text>
				</view>				
			</view>
		</view>
	</view>
</template>

<script>
	import iheader from '../../components/header.vue'

	export default {
		components: {
			iheader
		},
		data() {
			return {
				info:  JSON.parse(uni.getStorageSync('info')),
				avatar_path: uni.getStorageSync('avatar_path'),
				now: new Date().getFullYear(),
			}
		},
		onLoad(op) {
			if(wx.cloud){
				wx.showShareMenu({
					withShareTicket: true,
					menus: ['shareAppMessage', 'shareTimeline']
				})
			}
		},
		onShow() {
			this.initUser()
		},
		methods: {
			initUser: function() {
			}
		}
	}
</script>

<style scoped>
	page,
	.page {
		background-color: #f6f6f6;
	}

	.row {
		background-color: #fff;
		margin-bottom: 10upx;
		padding: 30upx;
	}

	.row:last-of-type {
		margin-bottom: 0upx;
	}

	.user {
		text-align: center;
		display: block;
		width: 100%;
	}

	.month-reading {
		border-left: 1px solid #f1f1f1;
		border-right: 1px solid #f1f1f1;
	}

	.user .text-muted {
		text-align: center;
	}

	.col-title {
		margin-bottom: 1px;
		color: #666;
	}

	.col-title navigator {
		display: inline-block;
	}

	.username {
		margin: 15upx auto;
	}

	.user image {
		width: 180upx;
		height: 180upx;
		border-radius: 90upx;
		border: 1px solid #ddd;
		margin: 0 auto;
		display: block;
	}

	.base-info image {
		margin-right: 30upx !important;
	}

	.base-info image.pull-right {
		margin-right: 0 !important;
		top: 0;
	}

	.base-info {
		padding-top: 0upx;
		padding-bottom: 0upx;
	}

	.base-info .col-12 {
		border-bottom: 1px solid #f6f6f6;
		padding-bottom: 30upx;
		padding-top: 30upx;
		color: #666;
	}

	.base-info .col-12:last-of-type {
		margin-bottom: 0;
		border-bottom: 0;
	}

	.user-intro {
		width: 100%;
		text-align: center;
	}

	.user-func {
		line-height: 40upx;
		border: 1px solid #000000;
	}

	.user-func .func-item {
		line-height: 300%;
		border-bottom: 1upx dashed #f1f1f1;
		padding: 0;
		margin: 0;
	}

	.user-func .color-red {
		margin-right: 8upx;
	}

	.reading-time text {
		margin: auto 8upx;
		color: red;
	}

	.reading-time .col-4 {
		box-sizing: border-box;
	}

	.reading-time .col-4:nth-child(1),
	.reading-time .col-4:nth-child(2),
	.reading-time .col-4:nth-child(3) {
		border-bottom: 1px solid #f6f6f6;
		padding-bottom: 16upx;
	}

	.reading-time .col-4:nth-child(4),
	.reading-time .col-4:nth-child(5),
	.reading-time .col-4:nth-child(6) {
		padding-top: 16upx;
	}

	.reading-time .col-4:nth-child(2),
	.reading-time .col-4:nth-child(5) {
		border-left: 1px solid #f6f6f6;
		border-right: 1px solid #f6f6f6;
	}

	image.me-icon {
		width: 40upx;
		height: 40upx;
		vertical-align: middle;
		margin-right: 6upx;
		position: relative;
		top: -4upx;
	}

	.signed image {
		width: 25upx;
		height: 25upx;
	}

	.signed.text-muted {
		color: #CCCCCC;
	}

	.col-title .text-right .line {
		color: #EFEFEF;
		margin: 0 10px;
	}

	.col-title .text-right>view {
		display: inline-block;
	}

	@media (min-width: 768px) {

		.base-info image,
		image.me-icon {
			max-width: 20px;
			max-height: 20px;
			top: -2upx;
		}

		.base-info .col-12 {
			padding-top: 25px;
			padding-bottom: 25px;
		}
	}
</style>
