<template>
    <div>
      <nav-header></nav-header>
      <nav-bread></nav-bread>
      <div class="accessory-result-page accessory-page">
        <div class="container">
          <div class="filter-nav">
            <span class="sortby">Sort by:</span>
            <a href="javascript:void(0)" class="default cur">Default</a>
            <a href="javascript:void(0)" class="price">Price <svg class="icon icon-arrow-short"><use xlink:href="#icon-arrow-short"></use></svg></a>
            <a href="javascript:void(0)" class="filterby stopPop" @click="showFilterPop">Filter by</a><!-- 点击弹出遮罩 filterby类 小屏时会出现 -->
          </div>
          <div class="accessory-result">
            <!-- filter -->
            <div class="filter stopPop" id="filter" :class="{'filterby-show':filterBy}"><!-- 小屏时的做法 -->
              <dl class="filter-price">
                <dt>Price:</dt>
                <dd><a href="javascript:void(0)" :class="{'cur':priceChecked=='all'}" @click="priceChecked='all'">All</a></dd> <!-- 默认选中all -->
                <dd v-for="(price,index) in priceFilter"> <!-- 把索引给flag -->
                  <a href="javascript:void(0)" @click="setPriceFilter(index)" v-bind:class="{'cur':priceChecked==index}" >{{price.startPrice}} - {{price.endPrice}}</a>
                </dd>
              </dl>
            </div>

            <!-- search result accessories list -->
            <div class="accessory-list-wrap">
              <div class="accessory-list col-4">
                <ul>
                  <li v-for="(item,index) in goodsList">
                    <div class="pic">
                      <!--图片懒加载v-lazy 替换 v-bind:src  -->
                      <a href="#"><img v-lazy="'static/'+item.productImg" alt=""></a>
                    </div>
                    <div class="main">
                      <div class="name">{{item.productName}}</div>
                      <div class="price">{{item.productPrice}}</div>
                      <div class="btn-area">
                        <a href="javascript:;" class="btn btn--m">加入购物车</a>
                      </div>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="md-overlay" v-show="overLayFlag" @click="closePop"></div>
      <nav-footer></nav-footer>
    </div>
</template>
<script>
     import './../assets/css/base.css'
     import './../assets/css/product.css'
     import NavHeader from '@/components/NavHeader'
     import NavBread from '@/components/NavBread'
     import NavFooter from '@/components/NavFooter'
     import axios from 'axios'
    export default{
        data(){
            return {
              goodsList:[],
              priceFilter:[
                {
                  startPrice:'0.00',
                  endPrice:'100.00'
                },
                 {
                  startPrice:'100.00',
                  endPrice:'500.00'
                },
                {
                  startPrice:'500.00',
                  endPrice:'1000.00'
                },
                {
                  startPrice:'1000.00',
                  endPrice:'2000.00'
                },
              ],
              priceChecked:'all',
              filterBy:false,//定义小屏价格窗口是否弹出
              overLayFlag:false //遮罩
            }
        },
        components:{
          NavHeader,
          NavFooter,
          NavBread
        },
        mounted:function () {
          this.getGoodsList();
        },
        methods:{
          getGoodsList(){
            axios.get('/api/goods').then((result)=>{
              var res = result.data;
              console.log(res);
              this.goodsList =res.result;
            })
          },
          //显示filterby的小屏右侧划窗显示价格选择
          showFilterPop() {
            this.filterBy = true;
            this.overLayFlag = true;
          },
          //选中价格区间，点击关闭遮罩
          setPriceFilter(index) {
            this.priceChecked = index;
            this.closePop();
          },
          //关闭遮罩
          closePop() {
            this.filterBy = false;
            this.overLayFlag = false;
          }
        }
    }
</script>
