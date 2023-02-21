<template>
	<view>
	<view  @click="SearchBoxHandler" class="my-search-container"  :style="{'background-color':bgcolor}">
	    <!-- 使用 view 组件模拟 input 输入框的样式 -->
	    <view class="my-search-box"  :style="{'border-radius': radius + 'px'}" >
	      <uni-icons type="search" size="17"></uni-icons>
	      <text class="placeholder">搜索</text>
	    </view>
	  </view>
	</view>
</template>

<script>
	export default {
		name:"my-search",
		data() {
			return {
				
			};
		},
		props:{
		bgcolor:{
			type:String,
			default: "#c00000"
		}	,
			
		radius :{
		type: Number,
		    // 单位是 px
		    default: 18	
		}
		},
			
		methods:{
			SearchBoxHandler(){
				console.log(222)
				this.$emit("myclick")
			}
		}
	}
</script>

<style lang="scss">
.my-search-container{
	 // background-color: #c00000;
	  height: 50px;
	  padding: 0 10px;
	    display: flex;
	    align-items: center;
	
	.my-search-box{
		height: 36px;
		 width: 100%;
		 // border-radius: 15px;
		 display: flex;
		   align-items: center;
		   justify-content: center;
		background-color: #FFFFFF;
		.placeholder{
			   font-size: 15px;
			    margin-left: 5px;
		}
	}
	
}
</style>