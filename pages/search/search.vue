<template>
	<view class="page" style="position: relative;">
		<view class="search-box bg-white" style="position: fixed;">
			<input class="input" type="text" placeholder="大家都在搜 林宥嘉" @input="input" placeholder-style="color:#ababab;" />
		</view>
		<view style="height: 90rpx;"></view>
		<!-- 热搜列表 -->
		<view class="padding-sm bg-white">
			<block v-for="(item,index) in musicList" :key="index">
				<view class="flex " @click="playSong(item)">
					<view class="flex xh align-center justify-center text-df text-gray">
						<view class="search-rank">
							{{index + 1}}
						</view>
					</view>
					<view class="flex flex-sub">
						<view class="padding-top-xs padding-bottom-xs">
							<view class="text-lg">{{item.name}}</view>
							<view class="text-sm text-gray" v-for="(sItem,sIndex) in item.artists" :key="sIndex">
								{{sItem.name}}
							</view>
						</view>
					</view>
					<view class="flex align-center">
						<text class="cuIcon-more"></text>
					</view>
				</view>
			</block>
		</view>
		<!-- last -->
		<view class="last">
			没有啦&nbsp;~
		</view>
	</view>
</template>

<script>
	import {
		mapState
	} from "vuex"
	export default {
		data() {
			return {}
		},
		computed: {
			...mapState(["musicList"])
		},
		methods: {
			input(e) {
				console.log(e.detail.value)
				let ssWords = e.detail.value
				this.$store.dispatch("getMusicList", ssWords)
			},
			playSong(item) {
				let {
					name,
					id
				} = item
				uni.navigateTo({
					url: "../play/play?id=" + id + "&name=" + name
				})
			},
		}
	}
</script>

<style>
	@import url("./index.css");

	.input {
		background-color: #e8e8e8e;
	}
</style>
