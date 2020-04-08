<template>
	<view class="buttonArea">
		<button type="default" :style="{margin, fontSize, width, height}" @click="$emit('card-click')">{{isShow ? title : cover}}</button>
	</view>
</template>

<script>
	export default {
		props: {
			margin: {
				type: String,
				default : '0px'
			},
			fontSize: {
				type: String,
				default: '24px'
			},
			title: {
				type: String,
				default: ''
			},
			cover: {
				type: String,
				default: ''
			},
			isShow: {
				type: Boolean,
				default: false
			},
			width: {
				type: String,
				default: '48px'
			},
			height: {
				type: String,
				default: '48px'
			}
		},
		methods: {
			
		},
	}
</script>

<style>
	.buttonArea button {
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
