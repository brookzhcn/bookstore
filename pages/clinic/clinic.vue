<template>
  <div class="home-container">
    <div class="content">

      <div class="section">
        <div class="l"></div>
        <div class="m">附近诊所</div>
        <div class="r"></div>
      </div>
      <div class="category-list">
        <div class="filter-bar">
			<button class='item' @click="sortedshopidList">综合排序</button>
			<button class='item' @click="sortedshopList">评分最高</button>
			<button class='item' @click="sortedshopListreverse">评分最低</button>
            <span>{{item.title}}</span>
          <!-- <div class="item" v-for="(item,index) in filterList" :key="index" >
            <i class="icon" :class='item.icon'></i>
          </div> -->
        </div>
        <div class="item-list">
   
          <div class="item-b" v-for="(item,index) in shopList" :key="index" @click="shoppingCartClick">
            <div class="item-l">
              <img :src="item.pic_url" >
              <!-- <img class="tag" :src="item.poi_promotion_pic" alt=""> -->
            </div>
            <div class="item-r">
              <div class="r-t">
                <span class="shop-name">{{item.name}}</span>
                <div class="t-c">
                  <div class="c-l">
                    <!-- 五角星 -->
                    <div class="l-l">
                      <i class="icon mt-star-s" v-for="(its,ids) in stars" :key="ids"></i>
                    </div>
                    <!-- 评分 -->
                    <div class="l-m">{{item.wm_poi_score}}</div>
                    <!-- 销量 -->
                    <div class="l-r">{{item.month_sales_tip}}</div>
                  </div>
                  <!-- 时间和距离 -->
                  <div class="c-r">
                    <!-- <span class="r-l">{{item.delivery_time_tip}}</span> -->
                    <!-- <div class="r-mi"></div> -->
                    <span class="r-r">{{item.distance}}</span>
                  </div>
                </div>
              </div>
              <div class="r-m">
                <span class="m-l">{{item.doctor}}</span>
                <div class="m-m"></div>
                <span class="m-r">{{item.nurse}}</span>
                <div class="m-m"></div>
                <span class="m-r">{{item.environment}}</span>
              </div>
             <div class="r-b">
				 <span class="b-l">{{item.business_time}}</span>
                <!-- <span class="b-l">支持自取</span> -->
                <!-- <span class="b-r">极速配送</span> -->
              </div>
              <div class="activi-c">
                <div class="ac-item" v-for="(itm,idx) in item.discounts2" :key="idx">
                  <div class="ac"  v-if="hasLogin === true">
                    <img class="ac-l" :src="itm.icon_url" alt="">
                    <span class="ac-r">{{itm.info}}</span>
				  </div>
				  <div class="ac"  v-else-if="hasLogin === false">
				  </div>
                  </div>
                </div>
              </div>
            </div>
			
			
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {mapState} from 'vuex';
import {homeData} from './data';
export default {
  data(){
    return{
	  orderType:0,
      shopList: [],
      filterList: [
       {
        title: '综合排序',
       },
       {
         title: '距离最近'
       },
      ],
	  filterListChecked:[],
	  filterArr:[],
      stars: [1,2,3,4,5]
    }
  },
  computed: {
  	...mapState(['hasLogin']),
	// sortedshopList: function() {
	// 	if(orderType){
	// 		shopList.sort(function(a,b){
	// 			if(orderType===1){
	// 				return b.wm_poi_score-b.wm_poi_score;
	// 			}else{
	// 				return a.wm_poi_score-b.wm_poi_score;
	// 			}
	// 		})
	// }
	// return shopList;
	// }
	
	// sortedshopList: function() {
	// 	function compare(a, b) {
	// 	if (a.wm_poi_score < b.wm_poi_score)
	// 		return 1;
	// 	if (a.wm_poi_score > b.wm_poi_score)
	// 		return -1;
	// 	return 0;
	// }
	// 	return this.shopList.sort(compare);
	// },
	// 
	
	// filtershop(){
	// 	
	// filterArr = shopList
	// 	
	// if(orderType) {
	// 	  filterArr.sort(function (p1, p2) {
	// 		if(orderType === 1) { 
	// 		  return p2.wm_poi_score - p1.wm_poi_score;
	// 		} else { 
	// 		  return p1.wm_poi_score - p2.wm_poi_score;
	// 		}
	// 	  })
	// 	  return filterArr;
	// 	}
	// },	
	
	
	// sortshoplistreverse: function() {
	// 	function compare(a,b) {
	// 		if (a.wm_poi_score < b.wm_poi_score)
	// 			return -1;
	// 		if (a.wm_poi_score > b.wm_poi_score)
	// 			return 1;
	// 		return 0;
	// 	}
	// 	return this.filterListCheckedreverse.sort(compare);
	// },
	
  },
  mounted(){
    this.shopList = homeData.homeList.data.poilist
  },
  methods: {
    shoppingCartClick() {
      wx.navigateTo({url: '/pages/shoppingcart/shoppingcart'})
    },
	setOrderType() {
		this.orderType = orderType
	},
	sortedshopList: function() {
		function compare(a, b) {
		if (a.wm_poi_score < b.wm_poi_score)
			return 1;
		if (a.wm_poi_score > b.wm_poi_score)
			return -1;
		return 0;
	}
		return this.shopList.sort(compare);
	},
	sortedshopListreverse: function() {
		function compare(a, b) {
		if (a.wm_poi_score < b.wm_poi_score)
			return -1;
		if (a.wm_poi_score > b.wm_poi_score)
			return 1;
		return 0;
	}
		return this.shopList.sort(compare);
	},
	sortedshopidList: function() {
		function compare(a, b) {
		if (a.id < b.id)
			return -1;
		if (a.id > b.id)
			return 1;
		return 0;
	}
		return this.shopList.sort(compare);
	}
  },
}
</script>

<style scoped>
.home-container{
  
}
.content{
  display: flex;
  flex-direction: column;
  position: relative;
}
.header{
  display: flex;
  align-items: center;
  height: 80rpx;
  position: fixed;
  width: 100%;
  padding: 0 30rpx;
  box-sizing: border-box;
  background-color: #fff;
  z-index: 999;
} 
.header-l{
  display: flex;
  align-items: center;
}
.header-l span{
  font-size: 28rpx;
  color: #000;
  margin: 0 10rpx;
}
.header-r{
  display: flex;
  align-items: center;
  flex: 1;
  background-color: #eee;
  height: 60rpx;
  border-radius:30rpx;
  margin-left: 30rpx;
}
.header-r i{
  color: darkgray;
  font-size: 32rpx;
  margin-left: 20rpx;
}
.header-r span{
  font-size: 24rpx;
  color: darkgray;
  margin-left: 10rpx;
}
/* 头部样式end */
.category-c{
  height: 360rpx;
  background-color: white;
  padding-top: 100rpx;
}
.category-c .grid-c{
  height: 340rpx;
  flex-wrap: wrap;
  display: flex;
}
.category-c .grid-c .item{
  width: 20%;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-sizing: border-box;
}
.category-c .grid-c .item img{
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
}
.category-c .grid-c .item span{
  font-size: 20rpx;
  color: #000;
  margin-top: 10rpx;
  width: 100%;
}
/* category样式end */
.ad-c{
  width: 100%;
  height: 200rpx;
  display: flex;
  background-color: #fff;
  box-sizing: border-box;
  padding:0 20rpx;
}
.ad-c .ad-img{
  width: 100%;
  height: 200rpx;
  background-size: cover;
}
.b-banner{
  display: flex;
  flex-direction: column;
  padding: 0 20rpx;
}
.recommended{
  display: flex;
  align-items: center;
  height: 160rpx;
  padding: 0 20rpx;
  margin: 20rpx 0;
}
.recommended img{
  height: 160rpx;
  width: 100%;
}
.hot-sale{
  display: flex;
  height: 160rpx;
  justify-content: space-between;
  background-color: #fff;
  padding: 0 20rpx;
}
.hot-sale .item{
  display: flex;
  flex-direction: column;
  width: 33.333%;
  height: 160rpx;
}
.hot-sale .item .img-c{
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  top: 0;
  left: 0;
}
.img-c img{
  width: 160rpx;
  height: 120rpx;
  border-radius: 8rpx;
}
.img-c span{
  display: flex;
  align-items: center;
  position: absolute;
  bottom: 0;
  background: rgba(0,0,0,.3);
  width: 160rpx;
  font-size: 24rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #fff;
}
.item .price{
  color: red;
  font-size: 24rpx;
  margin: 8rpx 0 0 30rpx;
}
.section{
  display: flex;
  align-items: center;
  margin: 20rpx;
  justify-content: center;
  box-sizing: border-box;
}
.l,
.r{
    height: 2rpx;
    width: 60rpx;
    background-color: darkgray;
  }
.m{
  font-size: 32rpx;
  color: #000;
  margin: 0 20rpx;
  font-weight: 700;
}
.category-list{
  display: flex;
  flex-direction: column;
}
.filter-bar{
  padding: 0 20rpx;
  display: flex;
  align-items: center;
  width: 100%;
  height: 70rpx;
  border-top: 2rpx solid #999;
  border-bottom: 2rpx solid #999;
}
.filter-bar .item{
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.filter-bar .item span,
.filter-bar .item .icon{
  font-size: 26rpx;
}
.filter-bar .item .icon{
  margin-left: 10rpx;
}
.item-list{
  display: flex;
  flex-direction: column;
  height: auto;
}
.item-list .item-h{
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 70rpx;
}
.item-list .item-h .item{
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #F8F8F8;
  flex: 1;
  margin: 0 20rpx;
  padding: 10rpx 0;
  color: #666;
}
.item-list .item-h .item span{
  font-size: 24rpx;
}
.item-list .item-b{
  display: flex;
  margin: 20rpx;
}
.item-b .item-l{
  width: 160rpx;
  height: 120rpx;
  position: relative;
  top: 0;
  left: 0;
}
.item-b .item-l img{
  width: 160rpx;
  height: 120rpx;
}
.item-b .item-l .tag{
  position: absolute;
  top: 0;
  left: 0;
  width: 100rpx;
  height: 60rpx;
}
.item-b .item-r{
  margin-left: 20rpx;
  flex-direction: column;
  flex: 1;
}
.item-r .r-t{
  display: flex;
  flex-direction: column;
}
.shop-name{
  font-size: 28rpx;
  color: #000;
  font-weight: 700;
}
.r-t .t-c,
.t-c .c-r{
  display: flex;
  align-items: center;
}
.t-c .c-l{
  display: flex;
  flex: 1;
}
.t-c .c-l .l-l{
  display: flex;
}
.t-c .c-l .l-l i{
  font-size: 20rpx;
  color: orange;
}
.t-c .c-l .l-m,
.t-c .c-l .l-r{
  font-size: 20rpx;
  margin-left: 20rpx;
}
.c-r .r-l,
.c-r .r-r{
 font-size: 20rpx;
}
.r-mi {
  width: 2rpx;
  height: 20rpx;
  background-color: gray;
  margin: 0 10rpx;
}
.r-m {
  display: flex;
  align-items: center;
  margin-top: 10rpx;
}
.r-m .m-l,.r-m .m-r {
  font-size: 20rpx;
}
.r-m .m-m {
  width: 2rpx;
  height: 20rpx;
  margin: 0 10rpx;
  background-color: gray;
}
.r-b {
  display: flex;
  align-items: center;
  margin-top: 10rpx;
}
.b-l,.b-r {
  color: #09CFB5;
  font-size: 20rpx;
  border: 2rpx solid #09CFB5;
  text-align: center;
  padding: 0 8rpx;
}
.b-r{
  margin-left: 10rpx;
}
.activi-c{
  display: flex;
  flex-direction: column;
}
.ac-item,.ac{
  display: flex;
  align-items: center;
  
}
.ac-item{
  margin-top: 20rpx;
}
.ac-l{
  width: 30rpx;
  height: 30rpx;
}
.ac-r{
  color: gray;
  font-size: 20rpx;
  margin-left: 10rpx;
}
</style>
