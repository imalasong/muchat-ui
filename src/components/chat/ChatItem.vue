<template>

	<div class="chat-item" :class="lineStyle()">
		<div class="left">
			<head-image :url="chat.headImage" :size="40" :id="chat.type=='PRIVATE'?chat.targetId:0"></head-image>
			<div v-show="chat.unreadCount>0" class="unread-text">{{chat.unreadCount}}</div>
		</div>
		<div class="mid">
			<div>{{ chat.showName}}</div>
			<div class="msg-text" v-if="chat.lastContent!=null" v-html="$emo.transform(chat.lastContent)"></div>
		</div>
		<div class="right ">
			<div @click.stop="onClickClose()"><i class="el-icon-close close" style="border: none; font-size: 20px;color: black;" title="关闭"></i></div>
			<div class="msg-time">
				<chat-time :time="chat.lastSendTime"></chat-time>
			</div>
		</div>
	</div>

</template>

<script>
	import ChatTime from "./ChatTime.vue";
	import HeadImage from '../common/HeadImage.vue';
  import chat from "@/view/Chat.vue";
	
	export default {
		name: "chatItem",
		components: {
			ChatTime,
			HeadImage
		},
		data() {
			return {}
		},
		props: {
			chat: {
				type: Object
			},
			active: {
				type: Boolean
			},
			index: {
				type: Number
			}
		},
    mounted() {
    },
    methods: {

      lineStyle(){
        let v = [];
        if (this.chat.type==='GROUP') {
          v.push("item-group")
        }
        if (this.active) {
          v.push("active");
        }

        return v;
      },
			onClickClose(){
        this.$http({
          url: "/chatSession/del",
          method: "DELETE",
          data: {
            "targetId":this.chat.targetId,
            "chatType":this.chat.type
          }
        }).then((data) => {
          this.$emit("del");
        })

			}
		}
	}
</script>

<style scode lang="scss">

	.chat-item {
		height: 65px; 
		display: flex;
		margin-bottom: 1px;
		position: relative;
		padding-left: 15px;
		align-items: center;
		padding-right: 5px;
		background-color: #fafafa;
		white-space: nowrap;

    &.item-group{
      background-color: rgba(206 63 202 / 2%);
    }
		
		&:hover {
			background-color: #eeeeee;
		}

		&.active {
			background-color: #3fcebd38;
		}


		&:hover {
			.close {
				display: block !important;
			}
		}
		
		.left {
			position: relative;
			display: flex;
			width: 45px;
			height: 45px;
			
			.unread-text {
				position: absolute;
				background-color: #f56c6c;
				right: -8px;
				top: -8px;
				color: white;
				border-radius: 30px;
				padding: 0 5px;
				font-size: 10px;
				text-align: center;
				white-space: nowrap;
				border: 1px solid #f1e5e5;
			}
		}


		.mid {
			margin-left: 15px;
			flex: 2;
			display: flex;
			flex-direction: column;
			height: 100%;
			flex-shrink: 0;
			overflow: hidden;

			&>div {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				flex: 1;
			}

			.msg-text {
				font-size: 14px;
				color: #888888;
				white-space: nowrap;
			}
		}

		.right {
			flex: 1;
			display: flex;
			flex-direction: column;
			align-items: flex-end;
			height: 100%;
			flex-shrink: 0;
			overflow: hidden;

			&>div {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				flex: 1;
			}

			.close {
				width: 1.5rem;
				height: 1.5rem;
				right: 0;
				top: 1rem;
				cursor: pointer;
				display: none;
			}

			.msg-time {
				font-size: 14px;
				color: #888888;
				white-space: nowrap;
			}
		}
	}

</style>
