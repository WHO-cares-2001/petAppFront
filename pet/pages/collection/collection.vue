<template>
	<view>
		<!-- 顶部导航栏 -->
		<TopBar barTitle="我的收藏" @click-left='callGoMy()'></TopBar>
		
		
		<!-- 商品内容 -->
		<view class="shop-list">
			<!-- <pet-card v-for="(item, index) in petList" :key="index" :pet="item" /> -->
			<view class="shop-item" v-for="(item,index) in petList" :key="index"
			>
				
				<image :src="petPath+item.img" mode="" class="shop-img"
				@tap="callGoDetail(item.animalId,item.goodsId,null)"></image>
				
				<view class="shop-text">
					<view class="" @tap="callGoDetail(item.animalId,item.goodsId,null)">
						<view class="shop-name">
							{{item.name}}
						</view>
						<view class="shop-option">
							<!-- {{item.option}} -->
						</view>
					</view>
					
					<view class="shop-price">
						<view class="">
							<!-- ￥{{item.pprice}} -->
						</view>
						<view class="">
							<uni-icons type="trash-filled" size="25" 
							color="#FE4355" @tap="del(item.id)"
							>
							</uni-icons>
						</view>
					</view>
				</view>
				
			</view>
		</view>
		
	</view>
</template>

<script>
import TopBar from '../../components/common/topBar.vue'
import {mapState,mapActions,mapGetters,mapMutations} from 'vuex'

import {goMy} from '@/common/sharedMethods.js'
import PetCard from '@/components/categorySelect/categoryList.vue'
import {
		getFavor,deleteFavor
	} from "../../network/modules/collection.js";
import {goDetail} from '@/common/sharedMethods.js'
	

export default {
	components:{
		TopBar,
		PetCard
	},
	data() {
		return {
			petList:[
				{
					checked:false,
					id:1,
					name:"啊拒绝偶爱你分配就怕看佛经破",
					option:"规格",
					imgUrl:"../../static/petImgs/img/16.jpg",
					price:"2000.0",
					num:1
				},
				{
					checked:false,
					id:2,
					name:"啊拒绝偶爱你分配就怕看佛经破",
					option:"规格",
					imgUrl:"../../static/petImgs/img/16.jpg",
					price:"2000.0",
					num:1
				},
				{
					checked:false,
					id:3,
					name:"啊拒绝偶爱你分配就怕看佛经破",
					option:"规格",
					imgUrl:"../../static/petImgs/img/16.jpg",
					price:"2000.0",
					num:1
				},
				{
					checked:false,
					id:4,
					name:"啊拒绝偶爱你分配就怕看佛经破",
					option:"规格",
					imgUrl:"../../static/petImgs/img/16.jpg",
					price:"2000.0",
					num:1
				},
				{
					checked:false,
					id:5,
					name:"啊拒绝偶爱你分配就怕看佛经破",
					option:"规格",
					imgUrl:"../../static/petImgs/img/16.jpg",
					price:"2000.0",
					num:1
				},
			],
			id:'',
			petPath:"../../static/petImgs/",
			// type:'',
			favorId:'',
			
		}
	},
	methods: {
		...mapMutations(['addfavorList','delfavorList']),
		
		del(id){
			let self=this
			
			deleteFavor(id)
			.then(function(res){
				console.log(res); // 输出完整的响应对象，以便查看数据结构
				  // if (res.status === 200) {
					self.$store.commit('delfavorList', id);

					console.log(res.data); 
					
				  // }
				// self.list=res.data
				
			})
		},
		goBack(){
			//优点：地址栏参数还是原先的
			uni.navigateBack({
				delta:1,//返回层数，2则上上页
			})
		},
		callGoMy(){
			console.log('触发')
			goMy();
		},
		getData(){
			let self=this
			this.id=localStorage.getItem('userId');
			console.log(this.id)
			
			getFavor(this.id)
			.then(function(res){
				console.log(res); // 输出完整的响应对象，以便查看数据结构
				 
					self.$store.commit('addfavorList', res.data)
					
					self.petList=res.data
					console.log(res.data); 
					
				 
				// self.list=res.data
				
			})
			.catch(function(err){
				console.log(err)
			})
		},
		callGoDetail(id,type,shopId){
			goDetail(id,type,shopId)
			console.log(id,type,shopId);
		
			// console.log(id)
		},
	},
	computed:{
		...mapState({
			list:state=>state.cart.list,
			
		})
	},
	mounted(){
		this.getData()
	}
}
</script>

<style scoped>
.shop-list{
	padding-bottom: 120rpx;
}

.shop-item{
	display: flex;
	padding: 20rpx;
	align-items: center;
	background-color: #F7F7F7;
	margin-bottom: 10rpx;
}

.shop-img{
	width: 200rpx;
	height: 200rpx;
	padding-left: 30rpx;
}

.shop-price{
	display:flex;
	justify-content: space-between;
}

.shop-option{
	font-size: 27rpx;
	color: #636263;
	padding:12rpx ;
}

.shop-text{
	flex:1;
	padding-left: 30rpx;
}

</style>
