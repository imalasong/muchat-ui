<template>
	<div class="friend-item" :class="active ? 'active' : ''">
		<div class="avatar">
			<head-image :url="friend.headImage"> </head-image>
		</div>
		<div class="text">
			<div>{{ friend.nickName}}</div>
			<div :class="online ? 'online-status  online':'online-status'">{{ online?"[在线]":"[离线]"}}</div>
		</div>
		<div v-if="showDelete" class="close" @click.stop="handleDel()">
			<i class="el-icon-close" style="border: none; font-size: 20px;color: black;" title="添加好友"></i>
		</div>
		<slot></slot>
	</div>
</template>

<script>
	import HeadImage from '../common/HeadImage.vue';

	export default {
		name: "frinedItem",
		components: {
			HeadImage
		},
		data() {
			return {}
		},
		methods:{
			handleDel(){
				this.$emit('del',this.friend,this.index)
			}
		},
		props: {
			friend: {
				type: Object
			},
			active: {
				type: Boolean
			},
			index: {
				type: Number
			},
			showDelete:{
				type: Boolean,
				default: true
			}
		},
		computed: {
			online() {
				return this.$store.state.friendStore.friends[this.index].online;
			}
		}
	}
</script>

<style scope lang="scss">
	.friend-item {
		height: 65px;
		display: flex;
		margin-bottom: 1px;
		position: relative;
		padding-left: 15px;
		align-items: center;
		padding-right: 5px;
		background-color: #fafafa;
		white-space: nowrap;
		&:hover {
			background-color: #eeeeee;
		}

		&.active {
			background-color: #dddddd;
		}


		.close {
			width: 1.5rem;
			height: 1.5rem;
			right: 10px;
			top: 1.825rem;
			cursor: pointer;
			display: none;
		}

		&:hover {
			.close {
				display: block;
			}
		}

		.avatar {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 45px;
			height: 45px;
		}

		.text {
			margin-left: 15px;
			flex: 1;
			display: flex;
			flex-direction: column;
			justify-content: space-around;
			height: 100%;
			flex-shrink: 0;
			overflow: hidden;

			&>div {
				display: flex;
				justify-content: flex-start;
			}

			.online-status {
				font-size: 12px;
				font-weight: 600;

				&.online {
					color: #5fb878;
				}
			}
		}
	}
</style>
