<template>
  <view>
   <view class="goods-list">
     <view v-for="(item, i) in goodsList" :key="i" @click="gotoDetail(item)">
       <!-- 为 my-goods 组件动态绑定 goods 属性的值 -->
       <my-goods :goods="item"></my-goods>
     </view>
   </view>
  </view>
</template>

<script>
	export default {
		data() {
			return {
				
				// 请求参数对象
				    queryObj: {
				      // 查询关键词
				      query: '',
				      // 商品分类Id
				      cid: '',
				      // 页码值
				      pagenum: 1,
				      // 每页显示多少条数据
				      pagesize: 10
				    },
					
					
					
				    // 商品列表的数据
				    goodsList: [],
					  defaultPic: 'https://img3.doubanio.com/f/movie/8dd0c794499fe925ae2ae89ee30cd225750457b4/pics/movie/celebrity-default-medium.png',
			           // isloading:false,
			};
		},
		
		onLoad(option) {
			this.queryObj.query=option.query  ||  ''
			this.queryObj.cid=option.cid  ||  ''
		// console.log(this.queryObj)
			 // 调用获取商品列表数据的方法
			  this.getGoodsList()
		},
		methods:{
		 // 获取商品列表数据的方法
		  async getGoodsList(cb) {
			    // this.isloading = true
		    // 发起请求
		    const { data: res } = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
		    if (res.meta.status !== 200) return uni.$showMsg()
			cb&&cb()
			// console.log(res.message)
		    // 为数据赋值
		     // 为数据赋值：通过展开运算符的形式，进行新旧数据的拼接
		    this.goodsList = [...this.goodsList, ...res.message.goods]
		    this.total = res.message.total
			  // this.isloading = false
		  },
		  gotoDetail(item){
		  			uni.navigateTo({
		  				 url: '/subpkg/good_detail/good_detail?goods_id=' + item.goods_id
		  			})  
		  }
		
		},
			
		onReachBottom(){
			
		  // 判断是否还有下一页数据
		  if (this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕！')
			// if(isloading)  return
			 //节流阀，不发起额外的请求
			this.queryObj.pagenum+=1
			this.getGoodsList()
		},
		
		//下拉刷新
		onPullDownRefresh() {
			// 1. 重置关键数据
			  this.queryObj.pagenum = 1
			  this.total = 0
			  this.isloading = false
			  this.goodsList = []
			  this.getGoodsList(()=>{
				  uni.stopPullDownRefresh()
			  })
			  
		}
		
	}
</script>

<style lang="scss">

</style>
