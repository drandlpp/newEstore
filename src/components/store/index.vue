<template>
    <div class="wrapper">
      <div class="search-container">
        <router-link to="/search"><span class="search-input"><i class="icon icon-search"></i><u>输入关键字</u></span></router-link><span class="search-button fr">搜索</span>
      </div>
      <swiper :options="swiperOption" ref="mySwiper" @someSwiperEvent="">
        <swiper-slide v-for="(sp, index) in swipeBox" :key="index"><img :src="imgUrl+sp.pic" alt="" class="swiper-image"></swiper-slide>
       <div class="swiper-pagination"  slot="pagination"></div>
      </swiper>
      <div class="index-classify">
        <ul>
          <li v-for="(cl, index) in claList" :key="index"><router-link to="/classify"><img :src="imgUrl+cl.logo" alt=""><span>{{cl.name}}</span></router-link></li>
          <li><router-link to="/classify"><img src="../../assets/images/all.png" alt=""><span>全部</span></router-link></li>
        </ul>
      </div>
      <div class="index-ad">
        <div class="ad-logo"></div>
        <div class="marquee_box">
         <!-- <ul class="marquee_list" :style="{ 'top': -num + 'rem'}" :class="{'marquee_top':num}">
            &lt;!&ndash;<li v-for="(item, index) in marqueeList" :key="index">{{item}}</li>&ndash;&gt;
            <li>{{marqueeList}}</li>
          </ul>-->
          <ul class="marquee_list">
            <li>{{marqueeList}}</li>
          </ul>
        </div>
        <div class="ad-more">更多 >></div>
        <!--<div class="ad-more"><router-link to="/announcement">更多 >></router-link></div>-->
      </div>
      <div class="floor-container">
        <div class="each-floor">
          <div class="floor-title ft1">
            <p>298专区</p>
            <span><router-link to="/zone/3">更多 >></router-link></span>
          </div>
          <div class="ft1-container">
            <ul>
              <li v-for="(mz, index) in moneyZone" :key="index">
                <router-link :to="/detail/+mz.id">
                  <img :src="imgUrl+mz.default_img" alt="">
                </router-link>
              </li>
            </ul>
            <div class="ft1-ad">
              <img src="../../assets/images/ft1_ad.png" alt="">
            </div>
          </div>
        </div>
        <div class="each-floor">
          <div class="floor-title ft3">
            <p>积分现金专区</p>
            <span><router-link to="/zone/2">更多 >></router-link></span>
          </div>
          <div class="ft2-container">
            <ul class="ft2-container-ul">
              <li v-for="(imz, index) in integralMoneyZone" :key="index">
                <router-link :to="/detail/+imz.id">
                  <img :src="imgUrl+imz.default_img" alt="">
                  <p class="goods-name">{{imz.name}}</p>
                  <div class="goods-price-integral">
                    <div>
                      <p class="spec"><i class="icon icon-x-integral"></i><span>{{imz.spec.points}}</span></p>
                      <u>+</u>
                      <p><span class="price-rmb">¥</span>{{imz.spec.ready}}</p>
                    </div>
                    <div>
                      <p class="spec"><i class="icon icon-y-integral"></i><span>{{imz.spec.points}}</span></p>
                      <u>+</u>
                      <p><span class="price-rmb">¥</span>{{imz.spec.ready}}</p>
                    </div>
                  </div>
                  <!--<p class="goods-price-rmb"><span>¥</span>139.60</p>-->
                </router-link>
              </li>
            </ul>
            <div class="goods-banner-container">
              <p class="goods-banner-title">
                <span class="banner-txt fl">品牌街</span>
                <span class="banner-more fr"><router-link to="/band">更多 >></router-link></span>
              </p>
              <ul class="goods-banner-list">
                <li v-for="(bd, index) in bandList" :key="index">
                  <router-link  :to="/bandZone/+bd.id">
                    <div class="goods-img-box"><img :src="imgUrl+bd.logo" :alt="bd.name"></div>
                    <p>{{bd.name}}</p>
                  </router-link>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <div class="each-floor">
          <div class="floor-title ft2">
            <p>积分专区</p>
            <span><router-link to="/zone/1">更多 >></router-link></span>
          </div>
          <div class="ft3-container">
            <ul class="ft2-container-ul">
              <li v-for="(iz, index) in integralZone" :key="index">
                <router-link :to="/detail/+iz.id">
                  <img :src="imgUrl+iz.default_img" alt="">
                  <p class="goods-name">{{iz.name}}</p>
                  <div class="goods-price">
                    <p><i class="icon icon-x-integral"></i><span>{{iz.spec.points}}</span></p>
                    <p><i class="icon icon-y-integral"></i><span>{{iz.spec.points}}</span></p>
                  </div>
                  <!--<p class="goods-price-rmb"><span>¥</span>139.60</p>-->
                </router-link>
              </li>
            </ul>

          </div>
        </div>
        <p style="text-align: center;color: #4e4c50;font-size: .2rem" onclick="window.location.href='http://www.miitbeian.gov.cn'"></p>
      </div>
      <tabFoot></tabFoot>
    </div>
</template>

<script>
  import tabFoot from '../comp/tabFoot.vue'
  import {swiper, swiperSlide } from 'vue-awesome-swiper'
  import api from '../../assets/js/api.js'
  import { imgUrl } from '../../assets/js/api.js'
  export default {
    name: "index",
    components: {
      tabFoot,
      swiper,
      swiperSlide
    },
    data() {
      return {
        swiperOption: {
          autoplay:true,
          loop: true,
          delay: 1000,
          // effect : 'fade',
          pagination: {
            el: '.swiper-pagination',
          },
        },
        num: 0,
        marqueeList: [
          '热烈庆祝新易家商城上线',
          '前一百位会员可享受购物优惠',
          '新易家，开启您的购物之旅'
        ],
        swipeBox: [],
        imgUrl:imgUrl,
        moneyZone: [],
        integralZone: [],
        integralMoneyZone: [],
        bandList: [],
        claList:[]
      }
    },
    computed: {
      swiper() {
        return this.$refs.mySwiper.swiper
      }
    },
    mounted() {
      this.swiper.slideTo(1, 1000, true)
    },
    methods: {
      showMarquee:function (num) {
        this.marqueeList.push(this.marqueeList[0]);
        var max = this.marqueeList.length;
        var that = this;
        var marqueetimer =  setInterval(function(){
          num++;
          if(num>=max ){
            num=0;
          }
          that.num=num*0.286;
        }, 3000);
      },
    },
    created () {
      this.showMarquee(this.num)
      api.swipe()
        .then(res => {
          this.swipeBox = res.data;
        });
      api.getZone({
        page: 1,
        type: 1
      })
        .then(res => {
          for (var i in res.data) {
            if (i <= 5) {
              this.integralZone.push(res.data[i])
            }
          }
        });
      api.getZone({
        page: 1,
        type: 2
      })
        .then(res => {
          for (var i in res.data) {
            if (i <= 5) {
              this.integralMoneyZone.push(res.data[i])
            }
          }
        });
      api.getZone({
        page: 1,
        type: 3
      })
        .then(res => {
          for (var i in res.data) {
            if (i <= 5) {
              this.moneyZone.push(res.data[i])
            }
          }
        });
      api.getBand()
        .then(res => {
          for (var i in res.data) {
            if (i <= 2) {
              this.bandList.push(res.data[i])
            }
          };
        })
      api.getClassify(0)
        .then(res => {
          for (var i in res.data) {
            if (i <= 3) {
              this.claList.push(res.data[i])
            }
          }
        });
      api.announcements()
        .then(res => {
          // console.log(res)
          this.marqueeList = res.data;
          // console.log(this.marqueeList)
        })
    },
  }
</script>

<style scoped lang="less">
  @import "../../assets/less/index.less";
</style>
