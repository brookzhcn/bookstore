<template>
  <div class="shopping-container">
    <div class="header-c">
      <div class="herder-t">
        <div class="h-l"><img :src="shopinfo.pic_url" alt="" class="shop-logo"></div>
        <div class="h-r">
        <!-- <div class="r-t">
            <span class="t-l">{{shopinfo.min_price_tip}}</span>
            <div class="s-l"></div>
            <span class="t-m">{{shopinfo.delivery_time_tip}}</span>
            <div class="s-l"></div>
            <span class="t-r">{{shopinfo.distance}}</span>
          </div> -->
          <div class="r-m">公告：{{shopinfo.bulletin}}</div>
          <div class="r-b">
            <span class="b-l"></span>
            <span class="b-r">{{discounts.info}}</span>
            <i class="icon mt-arrow-right-o"></i>
          </div>
        </div>
      </div>
      <div class="cate-c">
		<span class="c-l" :style="{'font-weight': pageIndex === 0 ? 'bold' : null}" @click="menuClick">科室</span>
		<span class="c-m" :style="{'font-weight': pageIndex === 1 ? 'bold' : null}" @click="commentClick">评价</span>
		<span class="c-r" :style="{'font-weight': pageIndex === 2 ? 'bold' : null}" @click="shopClick">商家</span>
        <div class="line"></div>
      </div>
    </div>
    <div class="list-c" v-if="pageIndex === 0">
     <scroll-view class="list-l" :scroll-y="true">
         <div class="l-item" :class="{active: index === tagIndex}" v-for="(item, index) in foods" :key="index" @click="categoryClick(item, index)">
          <img :src="item.icon" alt="" v-if="item.icon != ''">
          <span>{{item.name}}</span>
          <text class="count" v-if="item.count > 0">{{item.count}}</text>
        </div>
      </scroll-view>
      <scroll-view class="list-r" :scroll-y="true">
        <div class="item-list" v-for="(item,index) in foods" :key="index">
          <div class="section">
            <span class="s-title">{{item.name}}</span>
          </div>
          <div class="item" v-for="(its,ids) in item.spus" :key="ids">
            <div class="item-l"><img :src="its.picture" alt=""></div>
            <div class="item-r">
              <span class="r-title">{{its.name}}</span>
              <span class="sub-title">{{its.description}}</span>
              <span class="sale-num">{{its.month_saled_content}} {{its.praise_content}}</span>
              <div class="r-t">
                <span class="r-price">￥{{its.min_price}}</span>
                <div class="sku" v-if="its.attrs.length">
                  <span>选规格</span>
                  <span class="count" v-if="its.sequence >0">{{its.sequence}}</span>
                </div>
                <div class="add-item" v-else>
                  <div class="add-l" v-if="its.sequence>0" >
                    <i class="icon mt-reduce-o"></i>
                    <span>{{its.sequence}}</span>
                  </div>
                  <div class="add-r">
                    <i class="icon mt-add-o"></i>
                  </div>
                </div>
              </div>
              <div class="tags-c">
                <img class="tags" :src="itm.picture_url" v-for="(itm, idx) in its.product_label_picture_list" :key="idx">
              </div>
            </div>
          </div>
        </div>
      </scroll-view>
    </div>
	
	
	<div class="comment-c" v-else-if="pageIndex === 1">
      <scroll-view class="comment-sc" :scroll-y="true">
		  
        <!-- <div class="comment-header"  v-for="(commentInfo,index) in comments" :key="index"> -->
		<div class="comment-header">
          <div class="h-l">
            <span class="score">{{commentInfo.quality_score}}</span>
            <span class="title">商家评分</span>
          </div>
          <div class="h-m">
            <div class="m-t">
              <span class="title">口味</span>
              <div class="star-c">
                <i class="icon mt-star-s" v-for="(itx, idx) in stars" :key="idx"></i>
              </div>
              <span class="score">{{commentInfo.food_score}}</span>
            </div>
            <div class="m-b">
              <span class="title">包装</span>
              <div class="star-c">
                <i class="icon mt-star-s" v-for="(itx, idx) in stars" :key="idx"></i>
              </div>
              <span class="score">{{commentInfo.pack_score}}</span>
            </div>
          </div>
          <!-- <div class="line"></div>
          <div class="h-r">
            <span class="score">{{commentInfo.delivery_score}}</span>
            <span class="title">配送评分</span>
          </div> -->
        </div>
        <!-- <div class="comment-tags">
          <div class="tag-item" v-for="(item, index) in commentInfo.commentMolds" :key="index">
            <span>{{item}}</span>
          </div>
        </div> -->
        <div class="comment-list">
          <div class="item-c" v-for="(item, index) in commentInfo.comments" :key="index">
            <div class="item-l">
              <img :src="item.user_pic_url.length > 0 ? item.user_pic_url : 'http://ovyjkveav.bkt.clouddn.com/18-9-26/85572180.jpg'">
            </div>
            <div class="item-r">
              <div class="h-r">
                <div class="r-t">
                  <span class="name">{{item.user_name}}</span>
                  <span class="date">{{item.comment_time}}</span>
                </div>
                <div class="r-b">
                  <div class="b-l">
                    <i class="icon mt-star-s" v-for="(itx, idx) in stars" :key="idx"></i>
                  </div>
                  <!-- <span class="b-r">{{item.ship_time}}分钟送达</span> -->
                </div>
              </div>
              <div class="r-comtent">
                <span>{{item.comment}}</span>
              </div>
              <div class="r-imgs">
                <div class="single" v-if="item.comment_pics.length === 1">
                  <img :src="itm.url" v-for="(itm, idx) in item.comment_pics" :key="idx">
                </div>
                <div class="double" v-if="item.comment_pics.length === 2 || item.comment_pics.length === 3">
                  <img class="comment-img" :src="itm.url" v-for="(itm, idx) in item.comment_pics" :key="idx">
                </div>
                <div class="four" v-if="item.comment_pics.length === 4">
                  <img class="comment-img" :src="itm.url" v-for="(itm, idx) in item.comment_pics" :key="idx">
                </div>
              </div>
              <div class="reply-c" v-if="item.add_comment_list.length">
                <span>{{item.poi_reply_contents}}</span>
              </div>
            </div>
          </div>
        </div>
		
      </scroll-view>
    </div>
	
	
	
	<div class="shop-info" v-else-if="pageIndex === 2">
	      <div class="address">
	        <i class="icon mt-location-o"></i>
	        <span>上海市浦东新区滨江大道1616号</span>
	        <i class="icon mt-phone-o"></i>
	      </div>
	      <div class="delivery">
	        <div class="btm">
	          <i class="icon mt-clock-s"></i>
	          <span>预约电话：400 892 0288</span>
	        </div>
	      </div>
	    </div>


  </div>
</template>
<script>
import { mapState, mapActions, mapMutations, mapGetters } from "vuex";	
import {shoppingCart} from './data'
export default {
  data(){
    return {
	  tagIndex: 0,
	  pageIndex: 0,
      shopinfo: {},
      discounts: {},
      foods: [],
    }
  },
  mounted(){
    // console.log(shoppingCart.menuData);
    this.shopinfo = shoppingCart.menuData.data.poi_info
    console.log(this.shopinfo);
    this.discounts = this.shopinfo.discounts2[0]
    this.foods = shoppingCart.menuData.data.food_spu_tags
    console.log(this.foods);
  },
  computed: {
	  
  },
  methods: {
	  ...mapMutations("shoppingCart", ["changeReduceFeeDataMut", "changeSkuModalMut", "changeItemModalMut"]),
	  ...mapActions("shoppingCart", ["getMenuDataAction", "getCommentDataAction", "getCategoryMenuDataAction", "addItemAction", "reduceItemAction", "closeShoppingCartAction", "selectSkuAction", "changeSkuDataMut", "attrSelectAction", "changeSkuModalDataAction", "previewItemAction"]),
	categoryClick(item, index) {
	     this.tagIndex = index;
	     this.getCategoryMenuDataAction({index})
	   },
    menuClick() {
         this.left = 40 + 'rpx'
         this.pageIndex = 0
       },
    commentClick() {
         this.left = 182 + 'rpx'
         this.pageIndex = 1
         this.getCommentDataAction()
       },
    shopClick() {
         this.left = 325 + 'rpx'
         this.pageIndex = 2
       },
  }
}
</script>
<style scoped>
.shopping-container{
  display: flex;
  flex-direction: column;
  position: relative;
  top: 0;
  left: 0;
}
.header-c{
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
}
.herder-t{
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20rpx;
  box-sizing: border-box;
  background-color: #000;
  height: 150rpx;
}
.herder-t .h-l{
  width: 120rpx;
  height: 120rpx;
  display: flex;
}
.herder-t .h-l img{
  width: 100%;
  height: 100%;
  border-radius: 8rpx;
}
.herder-t .h-r{
  display: flex;
  flex-direction: column;
  margin: 0 30rpx 0 20rpx;
  font-size: 20rpx;
  color: #fff;
}
.herder-t .h-r .r-t,
.herder-t .h-r .r-m,
.herder-t .h-r .r-b{
  display: flex;
  align-items: center;
}
.r-t .s-l{
  margin: 0 20rpx;
  width: 2rpx;
  height: 30rpx;
  background-color: #fff;
}
.herder-t .h-r .r-m{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin: 10rpx 0;
  width: 575rpx;
}
.r-b .b-l{
  width: 30rpx;
  height: 30rpx;
  background-color: #FF616D;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: white;
  font-size: 20rpx;
}
.r-b  .b-r{
  margin-left: 10rpx;
}
.r-b .icon{
  font-size: 20rpx;
  color: white;
  margin-left: 240rpx;
}
.cate-c {
  display: flex;
  height: 70rpx;
  align-items: center;
  position: relative;
  transition: all 0.2s;
  background-color: #eee;
}
.cate-c .c-l,.cate-c .c-m,.cate-c .c-r {
  font-size: 32rpx;
  color: #000;
  margin-left: 40rpx;
}
.cate-c .line {
  position: absolute;
  width: 60rpx;
  height: 4rpx;
  background-color: orange;
  left: 40rpx;
  bottom: 0rpx;
  transition: left 0.2s;
}
.list-c {
  display: flex;
  position: fixed;
  top: 220rpx;
  bottom: 200rpx;
 }
.list-c .list-l {
  display: flex;
  flex-direction: column;
  width: 160rpx;
  background-color: #eee; 
}
.list-l .l-item{
  display: flex;
  width: 160rpx;
  align-items: center;
  justify-content: center;
  padding: 20rpx;
  box-sizing: border-box;
  position: relative;
  top: 0;
  left: 0;
}
.list-l .l-item img{
  width: 30rpx;
  height: 30rpx;
  border-radius: 50%;
}
.list-l .l-item span{
  font-size: 24rpx;
  color: #333;
  margin-left: 10rpx;
}
.list-l .l-item .count {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: red;
  width: 30rpx;
  height: 30rpx;
  border-radius: 15rpx;
  right: 0;
  top: 6rpx;
  position: absolute;
  font-size: 20rpx;
  color: white;
}
::-webkit-scrollbar {
  width: 0;
  height: 0;
  color: transparent;
}
::-webkit-scrollbar {
  display: none;
}
.list-r{
  display: flex;
  flex-direction: column;
  flex: 1;
  background-color: white;
}
::-webkit-scrollbar {
  width: 0;
  height: 0;
  color: transparent;
}
::-webkit-scrollbar {
  display: none;
}
.section {
  display: flex;
  height: 70rpx;
  align-items: center;
  margin-left: 20rpx;
}
.section .s-title {
  font-size: 28rpx;
  color: #000;
}
.item-list .item{
  display: flex;
  margin: 0 20rpx 30rpx;
}
.item .item-l{
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.item-l img{
  width: 160rpx;
  height: 160rpx;
  display: flex;
}
.item .item-r {
  display: flex;
  flex-direction: column;
  margin-left: 20rpx;
  justify-content: space-between;
  flex: 1;
}
.item .item-r .r-title {
  font-size: 28rpx;
  color: #000;
  font-weight: bold;
  overflow: hidden;
  line-height: 30rpx;
  height: 30rpx;
}
.sub-title {
  font-size: 20rpx;
  color: #333;
  line-height: 30rpx;
  overflow: hidden;
  height: 30rpx;
  margin-top: 10rpx;
}
.sale-num {
  font-size: 20rpx;
  color: #333;
  margin-top: 10rpx;
}
.item-r .r-t{
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.r-t .r-price{
  font-size: 32rpx;
  color: red;
  font-weight: 700;
}
.r-t .sku{
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: orange;
  padding: 8rpx 12rpx;
  border-radius: 25rpx;
  position: relative;
}
.sku span {
  font-size: 20rpx;
  color:#000
}
.sku .count {
  width: 30rpx;
  height: 30rpx;
  background-color: red;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  color: white;
  font-size: 20rpx;
  right: 0;
  top: -14rpx;
  border-radius: 15rpx;
}
.add-item{
  display: flex;
  align-items: center
}
.add-l {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.add-l i{
  font-size: 36rpx;
  color: gray;
}
.add-l span {
  font-size: 24rpx;
  color: #000;
  margin: 0 20rpx;
}
.add-r i {
   color: orange;
   font-size: 40rpx;
}
.tags-c{
  display: flex;
  align-items: center;
  margin-top: 10rpx;
}
.tags-c img{
  width: 60rpx;
  height:30rpx;
}



.comment-c .comment-sc {
  display: flex;
  position: fixed;
  top: 220rpx;
  flex-direction: column;
  height: 100%;
}
.comment .comment-sc .comment-header {
	margin-top: 20rpx;
	display: flex;
	align-items: center;
	height: 140rpx;
	background-color: white;
	width: 100%;
	justify-content: space-around;
}
.comment .comment-sc .comment-header .h-l {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.comment .comment-sc .comment-header .h-l .score {
            font-size: 50rpx;
            color: #CC0000;
}
.comment .comment-sc .comment-header .h-l .title {
	font-size: 20rpx;
	color: #000000;
}
.comment .comment-sc .comment-header .h-m {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}
.comment .comment-sc .comment-header .h-m .m-t {
	display: flex;
	align-items: center;
}
.comment .comment-sc .comment-header .h-m .m-t .title {
  font-size: 20rpx;
  color: #000000;
}
.comment .comment-sc .comment-header .h-m .m-t .star-c {
  display: flex;
  align-items: center;
  margin: 0 30rpx;
}
.comment .comment-sc .comment-header .h-m .m-t .star-c i {
  color: #FF3333;
  font-size: 24rpx;
}
.comment .comment-sc .comment-header .h-m .m-t .score {
              font-size: 24rpx;
              color: #FF3333;
}
.comment .comment-sc .comment-header .h-m .m-b {
            /* @extend .m-t; */
}
.comment .comment-sc .comment-header .line {
          width: 2rpx;
          height: 80rpx;
          background-color: #FF3333	;
          margin-left: 30rpx;
}
.comment .comment-sc .comment-header .h-r {
          /* @extend .h-l; */
}
.comment .comment-sc .comment-header .h-r .score {
            color: $textDarkGray-color
}
.comment .comment-sc .comment-list {
        margin-top: 20rpx;
        display: flex;
        flex-direction: column;
}
.comment .comment-sc .comment-list .item-c {
          display: flex;
          overflow: hidden;
          background-color: white;
          border-bottom: 2rpx solid #FF3333	;
}		  
.comment .comment-sc .comment-list .item-c .item-l {
            margin-left: 30rpx;
            margin-top: 20rpx;
}
.comment .comment-sc .comment-list .item-c .item-l img {
              width: 70rpx;
              height: 70rpx;
              border-radius: 35rpx;
}
.comment .comment-sc .comment-list .item-c .item-r {
            display: flex;
            flex-direction: column;
            background-color: white;
            margin-left: 20rpx;
            margin-top: 20rpx;
            margin-right: 30rpx;
            flex: 1;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r {
              display: flex;
              flex-direction: column;
              flex: 1;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-t {
                display: flex;
                justify-content: space-between;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-t .name {
                  font-size: 32rpx;
                  color:  #000000;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-t .date {
                  font-size:20rpx;
                  color: #CCCCCC;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-b {
                display: flex;
                align-items: center;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-b .b-l {
                  display: flex;
                  align-items: center;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-b .b-l i {
                    font-size: 20rpx;
                    color: ;
}
.comment .comment-sc .comment-list .item-c .item-r .h-r .r-b .b-r {
                  font-size:20rpx;
                  color: #CCCCCC;
                  margin-left: 20rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .r-comtent {
              display: flex;
              margin-top: 10rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .r-comtent span {
                font-size: 24rpx;
                color: #000000;
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs {
              display: flex;
              flex-direction: row;
              margin-top: 10rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs .single {
                margin-top: 10rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs .single img {
                  width: 300rpx;
                  height: 300rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs .double {
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs .double img {
                  width: 160rpx;
                  height: 160rpx;
                  margin-right: 16rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs .four {
                display: flex;
                width: 300rpx;
                flex-wrap: wrap;
                justify-content: space-between;
}
.comment .comment-sc .comment-list .item-c .item-r .r-imgs .four img {
                  width: 140rpx;
                  height: 140rpx;
                  margin: 10rpx 0;
}
.comment .comment-sc .comment-list .item-c .item-r .reply-c {
              display: flex;
              background-color: #F4F4F4;
              padding: 20rpx 14rpx;
              margin-top: 20rpx;
              margin-bottom: 30rpx;
}
.comment .comment-sc .comment-list .item-c .item-r .reply-c span {
                color: #CCCCCC;
                font-size: 24rpx;
}
.comment .comment-sc .comment-list .item-c:last-child {
          margin-bottom: 220rpx;
}  
   

.shop-info {
    display: flex;
    position: fixed;
    top: 220rpx;
    flex-direction: column;
    width: 100%;
    height: 100%;
}
.shop-info .address {
      display: flex;
      align-items: center;
      height: 70rpx;
      margin-top: 20rpx;
      background-color: white;
      padding: 0 20rpx;
}
.shop-info .address i {
        font-size: 38rpx;
        color: #CCCCCC;
      }
.shop-info .address span {
        flex: 1;
        margin: 0 20rpx;
        font-size: 24rpx;
        color: #000000;
}
.shop-info .delivery {
      display: flex;
      flex-direction: column;
      margin-top: 20rpx;
      background-color: white;
      padding: 0 16rpx;
}
.shop-info .delivery i {
          font-size: 32rpx;
          color: #CCCCCC;
}
.shop-info .delivery span {
          font-size: 24rpx;
          color: #000000;
          margin: 0 20rpx;
}
.shop-info .delivery i span btm {
        /* @extend .top; */
        border-bottom: 0 solid #FFB6C1;
}
</style>