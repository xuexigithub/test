<template>
  <div class="wrap">
    <div ref="senceTop">
      <div class="header" ref="senceSecKill">
        <img src="../img/zqxq01@2x.png" alt="">
        <img src="../img/zqxq02@2x.png" alt="">
        <img src="../img/zqxq03@2x.png" alt="">
        <img src="../img/zqxq04@2x.png" alt="">
      </div>
      <!-- 秒杀模块 -->
      <swiper :options="swiperOption" class="swiper-self-wrap" :class="senceSecKillFixed == true ? 'isFixed' :''">
        <!-- index == secKillNum|| -->
        <swiper-slide v-for="(item,index) in dataList" :key="item.index" class="seckill-item " 
        :class="{active:(index == secKillNum)}" @click.native="tapSecKill(item,index)" ref="mySwiper">
          <p class="topp">{{item.date}}</p>
          <p class="bottomp">{{item.time}}</p>
        </swiper-slide>
      </swiper>
      <!-- 秒杀 -->
      <div :class="senceSecKillFixed == true ? 'topDistance' :''">
        <div class="s">
          <div v-for="(item,key,index) in secKillGoodsList" :key="item.index">
            <div v-if="index == secKillNum">
              <div v-for="(subitem) in item" :key="subitem.g_id" class="sec-wrapper">
                <div>
                  <div v-if="subitem.g_surfaceImg" class="sec-img" :style="{backgroundImage:'url('+subitem.g_surfaceImg.gi_img+')'}">
                  </div>
                  <div v-else class="sec-img" :style="{backgroundImage:'url(https://cdn.16988.cn/res/html/images/h5/zw.png)'}">
                  </div>
                  <div class="sec-right">
                    <h3>{{subitem.g_name}}</h3>
                    <p><span class="price">￥{{subitem.g_activityPrice}}</span><s>原价￥{{subitem.g_price}}</s></p>
                    <!-- (subitem.remainTime).toString() -->
                    <p class="time-left"><span v-if="subitem.secKillStatus !== 0">倒计时:<count-down :endTime="(subitem.remainTime).toString()" @timeNoleft="callbacks(subitem)" endText="已结束"></count-down>
                          </span></p>
                  </div>
                  <div v-if="subitem.secKillStatus == 0" class="buy-btn" style="background:#D7D7D7;color:#fff">
                    <span>未开始</span>
                  </div>
                  <div v-else-if="subitem.secKillStatus == 1" class="buy-btn" @click="goToSecDetail(subitem.g_id)">
                    <span>去抢购</span>
                  </div>
                  <div v-else-if="subitem.secKillStatus == 2" style="background:transparent;color:#D7D7D7;border-radius: 25px;border:1px solid #D7D7D7" class="buy-btn">
                    <span>已结束</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- 跳转详情 -->
      <div>
        <img @click="gotoList()" src="./../img/zqxq05@2x.png" alt="">
        <img style="margin-top: 15px;" src="./../img/zqxq07@2x.png" alt="">
      </div>
    </div>
    <div class="sence-tab">
      <div class="tab-li">
        <ul :class="senceTabLiFixed == true ? 'isFixed' :''">
          <li @click="tapSence(index)" :class="{active:index == num}" v-for="(item,index) in tabSence" :key="item.index">{{item}}</li>
        </ul>
      </div>
      <div v-for="(item,index) in sendObjArr" :key="item.index">
        <div v-if="index == num">
          <div class="send-title">
            <img :src="item.img" alt="">
          </div>
          <div class="tap-content">
            <p>
              {{item.introTop}}
            </p>
            <p v-if="item.introBottom">{{item.introBottom}}</p>
          </div>
        </div>
      </div>
    </div>
    <div  v-for="(giftGood,key) in giftGoodsList" :key="giftGood.index">
      
      <div v-if="2 == num&&key=='family'" class="goods-list-wrapper">
        <div v-for="item in giftGood" :key="item.g_id" class="goods-list">
          
          <div>
            <div @click="gotoDetail(item.g_id)">
              <!-- <img :src="item.g_surfaceImg.gi_img" /> -->
              <div class="gi-img" :style="{backgroundImage:'url('+item.g_surfaceImg.gi_img+')'}">
              </div>
            </div>
            <div class="goods-intro">
              <h2>{{item.g_name}}</h2>
              <!-- <p>{{item.categoryName}}</p> -->
              <div class="goods-intro-bottom">
                <div class="item" style="color:#AA6B40;font-size:14px;">￥{{item.g_price}}</div>
                <div class="item" v-if="item.isInCart==0"><span class="cart-btn" @click="addCart(item,item.g_id)">+购物车</span></div>
                <div class="item" v-else-if="item.isInCart==1"><span class="cart-btn joined">已加入</span></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="1 == num&&key=='friends'" class="goods-list-wrapper">
        <div v-for="item in giftGood" :key="item.g_id" class="goods-list">
        
          <div>
            <div @click="gotoDetail(item.g_id)">
              <!-- <img :src="item.g_surfaceImg.gi_img" /> -->
              <div class="gi-img" :style="{backgroundImage:'url('+item.g_surfaceImg.gi_img+')'}">
              </div>
            </div>
            <div class="goods-intro">
              <h2>{{item.g_name}}</h2>
              <!-- <p>{{item.categoryName}}</p> -->
              <div class="goods-intro-bottom">
                <div class="item" style="color:#AA6B40;font-size:14px;">￥{{item.g_price}}</div>
                <div class="item" v-if="item.isInCart==0"><span class="cart-btn" @click="addCart(item,item.g_id)">+购物车</span></div>
                <div class="item" v-else-if="item.isInCart==1"><span class="cart-btn joined">已加入</span></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="0 == num&&key=='spouse'" class="goods-list-wrapper">
        <div v-for="item in giftGood" :key="item.g_id" class="goods-list">
         
          <div>
            <div @click="gotoDetail(item.g_id)">
              <!-- <img :src="item.g_surfaceImg.gi_img" /> -->
              <div class="gi-img" :style="{backgroundImage:'url('+item.g_surfaceImg.gi_img+')'}">
              </div>
            </div>
            <div class="goods-intro">
              <h2>{{item.g_name}}</h2>
              <!-- <p>{{item.categoryName}}</p> -->
              <div class="goods-intro-bottom">
                <div class="item" style="color:#AA6B40;font-size:14px;">￥{{item.g_price}}</div>
                <div class="item" v-if="item.isInCart==0"><span class="cart-btn" @click="addCart(item,item.g_id)">+购物车</span></div>
                <div class="item" v-else-if="item.isInCart==1"><span class="cart-btn joined">已加入</span></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="back-top" @click="backToTop()">
      <span>返回顶部</span>
    </div>
    <div style="font-size:0">
      <img src="./../img/zqxq10@2x.png" alt="">
    </div>
    <div v-if="isInApp" class="fixed-cart" @click="gotoCart()">
      <div>
        <img src="./../img/cart.png" alt="">
      </div>
      <div class="totalCount">
        <span>{{cartCount}}</span>
      </div>
    </div>
  </div>
</template>
<script>
  import {
    Group,
    XInput,
    Loading
  } from 'vux';
  import {
    swiper,
    swiperSlide
  } from "vue-awesome-swiper";
  import {
    MyLocalStorage,
    getCookie
  } from "./../../../assets/js/cookie";
  import countDown from "./countDown";
  export default {
    components: {
      XInput,
      Group,
      Loading,
      countDown
    },
    data() {
      return {
        num: 0,
        secKillNum: 0, //切换秒杀的
        catnum: 0,
        isInApp: true, //是否在APP里
        listData: [], //
        tabSence: ['送爱人', '送朋友', '送家人'],
        isInCart: false,
        cartCount: 0, //购物车总数
        secKillGoodsList: [],
        senceTabLiFixed: false, //送朋友
        senceSecKillFixed: false, //秒杀地址
        sendObjArr: [{
          img: 'https://zhangwan-static.oss-cn-hangzhou.aliyuncs.com/h5/153717062253172.png',
          introTop: '生活需要仪式感，表白不能只流于形式。',
          introBottom: '感谢爱人相伴，你需要一款真挚的艺术品，经得起岁月磨洗，正如你们的爱恋，历久而弥新。',
        }, {
          img: 'https://zhangwan-static.oss-cn-hangzhou.aliyuncs.com/h5/153717063353066.png',
          introTop: '同心而共济,始终如一。知心好友难寻，更应倍加珍惜。',
          introBottom: '平日难得相聚，团圆应备好礼，把酒言欢之时，若有艺品相伴，岂不痛快惬意！',
        }, {
          img: 'https://zhangwan-static.oss-cn-hangzhou.aliyuncs.com/h5/153717062253171.png',
          introTop: '血浓于水的亲情，如温暖宽厚的臂膀，承载着我们所有的软弱和悲伤,离家奔波，许久未归，更应让最传统的艺品，传递最浓烈的深情。',
          introBottom: '',
        },  ],
        swiperOption: {
          slidesPerView: 4,
          spaceBetween: 10,
          freeMode: true,
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          },
        },
        dataList: [ {
          date: '9月20号',
          time: '10:00开抢',
          sortDate:20
        }, {
          date: '9月21号',
          time: '10:00开抢',
          sortDate:21
        }, {
          date: '9月22号',
          time: '10:00开抢',
          sortDate:22
        }, {
          date: '9月23号',
          time: '10:00开抢',
          sortDate:23
        },{
          date: '9月24号',
          time: '10:00开抢',
          sortDate:24
        }, ],
        giftGoodsList: [],
        familyData: [],
        friendsData: [],
        spouseData: [],
        flag: false,
        date:''
      };
    },
    computed: {},
    watch:{
      '$route':'scollSticky'
    },
    methods: {
      init() {
        const content_json = {
          "title": "跟掌玩一起穿越回古代，看看古人是怎么过中秋的！",
          "content": "回家的时候，把这份惊喜带上",
          "image": "https://zhangwan-picture-prod.oss-cn-hangzhou.aliyuncs.com/aliyun_oss/activity_images/201809/18/14295959343.png",
          "url": 'https://app.16988.cn/activity/index/nationalDayWx201810'
        };
        if (typeof(HandPlay) != "undefined") { //在掌玩中
          HandPlay.setShareInfo(JSON.stringify(content_json));
          this.uid = HandPlay.getUserId();
        }
        this.date = new Date().getDate() +1
      },
      // APP请求页面数据
      reFresh(){
        this.getCartCount()
      },
      // 秒杀切换
      tapSecKill(item,index) {
        // var index =(this.$refs.mySwiper.swiper.activeIndex - 1) % length;
        console.log(index)
        this.secKillNum = index;
        this.date==item.sortDate
        // this.getIndexList()
      },
      // 场景切换
      tapSence(index) {
        this.num = index;
        // this.getBottomList(index)
      },
      // 跳转中秋list
      gotoList() {
        this.$router.push({
          path: './list'
        })
      },
      // 跳转购物车
      gotoCart() {
        if (typeof HandPlay != "undefined") {
          // HandPlay.setShareInfo(JSON.stringify(content_json));
          var obj = {
            targetPage: "shoppingCart"
          };
          HandPlay.execNative(JSON.stringify(obj));
          if (!this.uid) {
            //用户在掌玩中未登录
            HandPlay.execNative('{"targetPage":"login"}'); //跳转掌玩登录页面
          } 
        } else if (MyLocalStorage.Cache.get("u_id")) {
          // this.$router.push({
          //   path: "/cartListe"
          // });
          window.location.href = '/html/apph5/myShop.html#/cartListe'
        } else {
          this.$router.push({
            path: "/register",
          });
        }
      },
      // sendtab送家人 送朋友 送爱人  && 秒杀列表
      getBottomList(index) {
        this.$indicator.open('')
        this.$axios
          .post("/activity/index/nationalDayGoods201810")
          .then(res => {
            console.log(res.data)
            if (res.data.error_type == 0) {
              this.secKillGoodsList = res.data.data.secKill
              this.giftGoodsList = res.data.data.gift
              this.$indicator.close()
            } else {
              this.$indicator.close()
              console.log(res.data.error_msg)
            }
          })
          .catch(function(error) {
            console.log(error);
          });
      },
      // 加入购物车
      addCart(item,id) {
        var that = this;
        if (typeof HandPlay != "undefined") {
          this.uid = HandPlay.getUserId();
          if (!this.uid) {
            //用户在掌玩中未登录
            HandPlay.execNative('{"targetPage":"login"}'); //跳转掌玩登录页面
          }else{
            this.$indicator.open('')
          this.$axios
            .post("/mall/user/cart/add?g_id=" + id + '&goodsNum=1')
            .then(function(response) {
              if (response.data.error_code == '0') {
                console.log(response.data.data);
                that.cartLists = response.data.data;
                // that.isInCart = true;
                // that.maskCart = false;
                // that.cartCount += parseInt(that.goodCount);
                that.$toast('已加入购物车');
                if (item.isInCart == 0) {
                  that.$set(item, "isInCart", 1);
                  console.log(item.isInCart);
                } else {
                  that.$set(item, "isInCart", 0);
                }
                that.getCartCount();
                that.$indicator.close()
              }
            })
            .catch(function(error) {
              console.log(error);
            });
          }
        }else{
          if (MyLocalStorage.Cache.get("u_id")) {
          this.$indicator.open('')
          this.$axios
            .post("/mall/user/cart/add?g_id=" + id + '&goodsNum=1')
            .then(function(response) {
              if (response.data.error_code == '0') {
                console.log(response.data.data);
                that.cartLists = response.data.data;
                // that.isInCart = true;
                // that.maskCart = false;
                // that.cartCount += parseInt(that.goodCount);
                that.$toast('已加入购物车');
                if (item.isInCart == 0) {
                  that.$set(item, "isInCart", 1);
                  console.log(item.isInCart);
                } else {
                  that.$set(item, "isInCart", 0);
                }
                that.getCartCount();
                that.$indicator.close()
              } else if (response.data.error_msg == '未登录') {
                that.$router.push({
                  path: '/register',
                  query: {
                    id: id
                  }
                });
                that.$indicator.close()
              } else {
                that.$toast(response.data.error_msg);
                that.isInCart = fasle;
                that.$indicator.close()
              }
            })
            .catch(function(error) {
              console.log(error);
            });
        } else {
          that.$router.push({
            path: '/register',
            query: {
              id: id
            }
          });
        }
        }
        
        
      },
      // 购物车数量
      getCartCount() {
        var that = this;
        this.$axios
          .post("/mall/user/cart/queryByPage?act=new")
          .then(function(response) {
            if (response.data.error_code == "0") {
              console.log(response.data.data);
              that.cartCount = parseInt(response.data.data.totalCount);
            } else {
              console.log(response);
            }
          })
          .catch(function(error) {
            console.log(error);
          });
      },
      // 跳转详情
      gotoDetail(id) {
        if (typeof HandPlay != "undefined") {
          var obj = {
            targetPage: "baby",
            gid: id
          };
          HandPlay.execNative(JSON.stringify(obj));
        } else {
          window.location.href = `/wx/wx/auth?type=goods&id=${id}`
        }
      },
      // 秒杀跳转
      goToSecDetail(id) {
        if (typeof HandPlay != "undefined") {
          var obj = {
            targetPage: "baby",
            gid: id
          };
          HandPlay.execNative(JSON.stringify(obj));
        } else {
          window.location.href = `/wx/wx/auth?type=goods&id=${id}`
        }
      },
      // 微信分享
      getWxMesg() {
        var that = this;
        this.$indicator.open('')
        this.$axios
          .post("/wx/wx/share", {
            url: location.href.split("#")[0]
          })
          .then(function(res) {
            that.$indicator.close()
            console.log(res.data.data);
            console.log("--------------------");
            wx.config({
              debug: false,
              appId: res.data.data.appId, // 和获取Ticke的必须一样------必填，公众号的唯一标识
              timestamp: res.data.data.timestamp, // 必填，生成签名的时间戳
              nonceStr: res.data.data.noncestr, // 必填，生成签名的随机串
              signature: res.data.data.signature, // 必填，签名，见附录1
              //需要分享的列表项:发送给朋友，分享到朋友圈
              jsApiList: ["onMenuShareAppMessage", "onMenuShareTimeline"]
            });
            //处理验证失败的信息
            wx.error(function(res) {
              console.log("验证失败返回的信息:", res);
              // alert(res)
            });
            //处理验证成功的信息
            wx.ready(function() {
              //分享到朋友圈
              var title = '跟掌玩一起穿越回古代，看看古人是怎么过中秋的！';
              var descs = '回家的时候，把这份惊喜带上';
              var imgurl = "http://zhangwan-picture-prod.oss-cn-hangzhou.aliyuncs.com/aliyun_oss/activity_images/201809/18/14295959343.png";
              var link = "https://app.16988.cn/activity/index/nationalDayWx201810";
              wx.onMenuShareTimeline({
                title: title,
                desc: descs,
                link: link,
                imgUrl: imgurl,
                success: function(res) {
                  // 用户确认分享后执行的回调函数
                  console.log("分享到朋友圈成功返回的信息为:", res);
                },
                cancel: function(res) {
                  // 用户取消分享后执行的回调函数
                  console.log("取消分享到朋友圈返回的信息为:", res);
                  console.log(res);
                }
              });
              //分享给朋友
              wx.onMenuShareAppMessage({
                title: title,
                desc: descs,
                link: link,
                imgUrl: imgurl,
                success: function(res) {
                  // 用户确认分享后执行的回调函数
                  console.log("分享到朋友圈成功返回的信息为:", res);
                },
                cancel: function(res) {
                  // 用户取消分享后执行的回调函数
                  console.log("取消分享到朋友圈返回的信息为:", res);
                  console.log(res);
                }
              });
            });
            that.$indicator.close()
          })
          .catch(function(error) {
            that.$indicator.close()
            console.log(error);
          });
      },
      // 返回顶部
      backToTop() {
        document.documentElement.scrollTop = document.body.scrollTop = 0;
        document.body.scrollTop = 0;
        document.documentElement.scrollTop = 0;
        window.pageYOffset = 0;
        document.documentElement.scrollTop = document.body.scrollTop = 0;
      },
      // 场景tab滚动固定
      scollSticky() {
        this.$nextTick(function() {
          var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
          var senceTop = this.$refs.senceTop.clientHeight
          var senceSecKill = this.$refs.senceSecKill.clientHeight
          if (scrollTop > senceSecKill) {
            console.log('顶部黏贴')
            this.senceTabLiFixed = false; //底部转态
            this.senceSecKillFixed = true;
          } else {
            this.senceSecKillFixed = false;
          }
          if (scrollTop > senceTop) {
            console.log('底部黏贴')
            this.senceSecKillFixed = false;
            this.senceTabLiFixed = true; //底部转态
          } else {
            this.senceTabLiFixed = false;
          }
        });
      },
      callbacks(subitem){
        // console.log(subitem)
        // this.$set()
        subitem.secKillStatus = 2
      },
    },
    created() {
      this.getBottomList();
      this.getCartCount();
      this.init();
      this.getWxMesg()
    },
    mounted() {
      var that = this;
      window.reFreshed = this.reFresh
      window.addEventListener('scroll', function addScroll() {
        if (that.flag) {
          return false;
        }
        that.flag = true;
        setTimeout(() => {
          that.scollSticky()
          that.flag = false;
        }, 50)
      });
      that.getWxMesg()
    },
    destroyed() {
      console.log("destroyed")
      window.removeEventListener('scroll', this.addScroll)
    },
  };
</script>

<style lang="scss" scoped>
  @import "../../../common/reset.css";
  @function pxToRem($px) {
    @return $px / 75px * 1rem;
  }
  @import "../../../common/swiper.min.css";
  body {
    overflow: hidden;
  }
  .header {
    font-size: 0;
    img {
      width: 100%;
    }
  }
  .wrap {
    background: #191005;
  }
  .sence-tab {
    background-size: 100% 100%;
    width: 100%;
    background-repeat: no-repeat;
    font-size: 14px;
  }
  .tap-content {
    color: #FFE6CB;
    text-align: center;
    margin-bottom: 20px;
    padding: 0 10px;
  }
  .tab-li {
    text-align: center;
    width: 98%;
    margin: 0 auto;
    padding-top: 20px;
    display: flex;
    // background: #333;
    ul {
      display: flex;
      width: 100%;
    }
    li {
      flex: 1;
      color: #fff;
      padding: 8px 7px;
      background-size: 100% 100%;
      margin: 0 5px;
    }
    .active {
      background: #F1C10A;
      color: #000;
      border-radius: 20px;
    }
  }
  .tab-cat-wrapper {
    border: 1px solid #d7a461;
    margin: 10px;
    border-radius: 25px;
    .active {
      color: #fff;
      background: #d7a461;
      border-radius: 25px;
    }
    .tab-cat {
      display: flex;
      text-align: center;
      li {
        flex: 1;
        padding: 10px;
      }
    }
  }
  .goods-list-wrapper {
    padding: 10px;
    display: flex;
    flex-flow: row wrap;
    justify-content: space-between;
    .goods-list {
      display: inline-block;
      width: 46%;
      padding: 5px;
      background: #fff;
      margin-bottom: 5px;
    }
  }
  .goods-intro {
    padding: 10px 0;
    h2 {
      font-size: 14px;
      overflow: hidden;
      text-overflow: ellipsis; //文本溢出显示省略号
      white-space: nowrap; //文本不会换行（单行文本溢出）
      width: 100%;
      margin-bottom: 15px;
    }
    p {
      font-size: 14px;
      color: #666;
      margin: 10px 0;
    }
    .goods-intro-bottom {
      display: flex;
      .cart-btn {
        background: #AA6B40;
        color: #fff;
        padding: 4px 10px;
        border-radius: 25px;
      }
      .joined{
        background:#fff;
        color:#AA6B40 ;
        border: 1px solid #aa6b40;
      }
      .item {
        flex: 1
      }
    }
  } // 秒杀模块
  .swiper-self-wrap {
    .topp {
      padding: 5px 10px;
      font-size: 14px;
    }
    .bottomp {
      font-size: 12px;
    }
    .active {
      .topp {
        background: #F1C10A;
        padding: 5px 10px;
        color: #000;
        border-radius: 20px;
      }
      .bottomp {
        color: #F1C10A;
      }
    }
  }
  @media screen and (max-width: 320px){
    .swiper-self-wrap {
    .topp {
      padding: 5px 10px;
      font-size: 14px;
    }
    .bottomp {
      font-size: 12px;
    }
    .active {
      .topp {
        background: #F1C10A;
        padding: 5px 10px;
        color: #000;
        border-radius: 20px;
      }
      .bottomp {
        color: #F1C10A;
      }
    }
  }
  }
  .seckill-tab {
    display: flex;
    text-align: center;
  }
  .seckill-item {
    // flex:1;
    display: inline-block;
    width: pxToRem(160px) !important;
    padding: 5px;
    color: #fff;
    text-align: center;
    margin-right: 0px !important;
  }
  @media screen and (max-width: 320px) {
    .seckill-item{
      width: pxToRem(180px) !important;
    }
  }
  .sec-wrapper {
    // margin-top: 10px;
    background: #fff;
    padding: 6px 15px;
    position: relative;
    border-top: 5px solid #000;
  }
  .sec-img {
    display: inline-block;
    width: 113px;
    height: 84px; // background: url('./../images/zuopin.jpg') center center;
    background-size: cover;
    background-position: center center;
    position: relative;
    span {
      position: absolute;
      right: 4px;
      bottom: 2px;
      color: #fff;
    }
  }
  .sec-right {
    display: inline-block;
    vertical-align: top;
    margin-left: 10px;
    position: relative;
    width: 50%;
    font-family: PingFang-SC-Medium;
    p {
      margin: 8px 0;
    }
    s {
      color: rgba(153, 153, 153, 1);
    }
    h3 {
      font-size: 16px;
      font-family: PingFang-SC-Medium;
      color: #4D1F0B;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      width: 90%;
    }
    .price {
      font-size: 18px;
      color: #AA6B40;
      margin-right: 8px;
      font-family: PingFang-SC-Medium;
    }
    .time-left {
      display: inline-block;
      background: rgba(248, 240, 231, 1);
      border-radius: 15px;
      color: rgba(206, 160, 104, 1);
      margin-left: -5px;
      span {
        padding: 2px 9px;
      }
    }
  }
  .buy-btn {
    position: absolute;
    right: 15px;
    bottom: 15px;
    background: #AA6B40;
    border-radius: 4px;
    color: #fff;
    font-size: 14px;
    padding: 2px 8px;
    border-radius: 20px;
  }
  .send-title {
    margin: 20px 0;
  }
  .back-top {
    text-align: center;
    background: #000;
    padding: 10px;
    span {
      padding: 6px 18px;
      background: #CFA166;
      color: #fff;
      border-radius: 20px;
      font-size: 16px;
    }
  }
  .fixed-cart {
    position: fixed;
    right: 20px;
    bottom: 50px;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 50%;
    display: flex;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    img {
      width: 30px;
      padding: 15px;
      vertical-align: middle;
    }
  }
  .totalCount {
    position: absolute;
    top: 6px;
    right: 6px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background: #E60012;
    width: 22px;
    height: 22px;
  }
  .totalCount span {
    color: #fff;
    font-size: 12px;
  }
  .gi-img {
    // width: pxToRem(340px);
    height: pxToRem(226px);
    background-size: cover;
    background-position: center center;
    position: relative;
  }
  .isFixed {
    position: fixed;
    top: 0;
    z-index: 99;
    background: #000;
    width: 100%;
    padding: 10px 0;
  }
  .topDistance {
    margin-top: 75px;
  }
</style>



