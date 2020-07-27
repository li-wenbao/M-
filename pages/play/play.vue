<template>
	<view class="content">
		<!-- <view class="status-bar"></view> -->
		<view class="text-white text-lg" style="margin-top: 50rpx;">
			{{muiscName}}
		</view>
		<!-- videodisc -->
		<view style="margin-top: 250rpx;">
			<view class="flex align-center justify-center round bg-black" :class="ifPlay===true?'imgrotate_common':''" style="width: 430rpx; height: 430rpx;position: relative;">
				<image class="flex align-center justify-center round bg-white black" style="width: 230rpx; height: 230rpx;position: absolute;" :src="muiscface">
				</image>
			</view>
			<!-- <image src="../../static/images/videodisc.png" mode=""></image> -->
		</view>
		<!-- optBar -->
		<view class="play-opt-bar">
			<view class="" v-for="(item, index) in optList" :key="index">
				<text class="text-white" :class="item.icon"></text>
			</view>
		</view>
		<!-- slider -->
		<view class="player-slider">
			<view class="player-currentTime">
				{{chCurrentTime[0]}}:{{chCurrentTime[1]}}
			</view>
			<slider class="slider" min="0" :max="duration" :value="currentTime" activeColor="#b6b6b6" backgroundColor="#dedede"
			 block-size="12" @change="changeProgress" />
			<view class="player-duration">
				{{chDuration[0]}}:{{chDuration[1]}}
			</view>
		</view>
		<!-- playbar -->
		<view class="play-bar">
			<view class="">
				<text class="text-white cuIcon-repeal"></text>
			</view>
			<view class="">
				<text class="text-white cuIcon-backwardfill"></text>
			</view>
			<view class="play-menu">
				<text class="text-white cuIcon-playfill" :class="ifPlay?'cuIcon-stop':'cuIcon-playfill'" @tap="playMusic"></text>
			</view>
			<view class="">
				<text class="text-white cuIcon-play_forward_fill"></text>
			</view>
			<view class="">
				<text class="text-white cuIcon-list"></text>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		mapState
	} from "vuex"
	const audioContext = uni.createInnerAudioContext();
	audioContext.autoplay = false;
	let currentTime ="", time = 0  
	export default {
		data() {
			return {
				optList: [{
					'id': 0,
					'icon': 'cuIcon-like'
				}, {
					'id': 1,
					'icon': 'cuIcon-down'
				}, {
					'id': 2,
					'icon': 'cuIcon-notice'
				}, {
					'id': 3,
					'icon': 'cuIcon-message'
				}, {
					'id': 4,
					'icon': 'cuIcon-moreandroid'
				}, ],
				duration: '100',
				currentTime: '0',
				chDuration: ['0', '00'],
				chCurrentTime: ['0', '00'],
				ifPlay: false,
				muiscName:""
			}
		},
		onLoad(options) {
			let {name,id} = options
			// console.log("+++++++++++++++++",name,id)
			this.muiscName = name
			// let id = options.id
			this.$store.dispatch("getMusic", id)
			this.$store.dispatch("musicFace", id)
			console.log("this.muisc",this.muisc)
			
		},
		computed: {
			...mapState(["muisc", "muiscface"]),
			
		},
		methods: {
			goBack() {
				history.back();
			},
			playMusic() {
				audioContext.src = this.muisc;
				var duration = audioContext.duration;
				currentTime = audioContext.currentTime;
				this.currentTime = currentTime;
				this.duration = duration;
				
				this.chDuration[0] = Math.floor(Math.floor(duration) / 60);
				this.chDuration[1] = Math.floor(duration) % 60;
				
				setInterval(()=>{
					time = time+1
					console.log(time)
					// this.chCurrentTime[0] = Math.floor(Math.floor(currentTime) / 60);
					// this.chCurrentTime[1] = Math.floor(currentTime) % 60;
				},1000)
				
				if (this.ifPlay) {
					this.ifPlay = false;
					audioContext.pause();
				} else {
					this.ifPlay = true;
					audioContext.play();
				}
			},
			changeProgress(e) {
				console.log("asfasg:+++++++++",e)
				audioContext.seek(e.detail.value);
				this.playMusic();
				// this.ifPlay = false;
			},
		}
	}
</script>

<style>
	@import url("./index.css");
</style>
