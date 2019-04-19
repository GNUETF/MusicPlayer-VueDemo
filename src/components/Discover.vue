<template>
    <div id="Discover">
      <div class="top">
        <swiper :options="swiperDiscover" ref="mySwiper" class="RotationChart">
          <swiper-slide v-for="(num,index) in 8 " :key='index'>
            <img :src="require('../assets/'+num+'.jpg') " alt="" height="100" width="100%">
          </swiper-slide>
          <div class="swiper-pagination"  slot="pagination"></div>
        </swiper>
      </div>
      <!--一排球-->
      <div class="optionCircular">
        <ul>
          <li>
            <div> <i class="fa fa-calendar-check-o" aria-hidden="true"></i> </div>
            <span>每日推荐</span>
          </li>
          <li>
            <div> <i class="fa fa-align-left" aria-hidden="true"></i> </div>
            <span>歌单</span>
          </li>
          <li>
            <div> <i class="fa fa-sort-amount-desc" aria-hidden="true"></i> </div>
            <span>排行榜</span>
          </li>
          <li>
            <div> <i class="fa fa-podcast" aria-hidden="true"></i> </div>
            <span>电台</span>
          </li>
          <li>
            <div> <i class="fa fa-caret-square-o-right" aria-hidden="true"></i> </div>
            <span>直播</span>
          </li>
        </ul>
      </div>
      <!--推荐歌单-->
      <div class="songSheet">
        <ul>
          <li><div><b>推荐歌单</b></div> <div>歌单广场</div></li>
          <li v-for="List in imgList">
            <!--{{List}}-->
            <div v-for="url in List">
              <!--{{'../assets/'+(url.url)+'.jpg'}}-->
              <img :src="require('../assets/'+(url.url)+'.jpg')" alt=""  width="100%" height="90">
              <p>{{url.text}}</p>
            </div>
          </li>
        </ul>
      </div>
      <!--云村精选-->
      <div class="cloudSelected">
        <div class="title">
          <div><b>云村精选</b></div>
          <div><i class="fa fa-undo" aria-hidden="true"></i> 获取新内容</div>
        </div>

        <ul>
          <li>
            <p>BTS&Halsey合作官方MV来了!!!</p>
            <div class="textImg">
              <img src="../assets/3.jpg" alt="" width="100%" height="150px">
              <div><b>Boy with Luv feat.Halsey Official MV</b></div>
            </div>
            <div class="operation">
              <div><i class="fa fa-thumbs-o-up" aria-hidden="true"></i>76942</div>
              <div><i class="fa fa-commenting-o" aria-hidden="true"></i>12623</div>
            </div>
          </li>
          <li>
            <p>#十里桃花# 只为一人</p>
            <div class="textImg">
              <img src="../assets/8.jpg" alt="" width="100%" height="150px">
              <div><b>在春风里荡漾, 花香盈袖,静看白云飘飘。 争花不待叶,密缀欲无条。 傍沼人窥鉴,惊鱼水溅桥</b></div>
            </div>
            <div class="operation">
              <div><i class="fa fa-thumbs-o-up" aria-hidden="true"></i>76942</div>
              <div><i class="fa fa-commenting-o" aria-hidden="true"></i>12623</div>
            </div>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
    export default {
        name: "Discover",
      data(){
          return{
            swiperDiscover: {
              pagination: {
                el: '.swiper-pagination'

              },
              autoplay:true,
              loop : true,
            },
            imgList:[
              {
                imgUrlA:{
                  url:'1',
                  text:'【VIP专享】，一周新歌推荐'
                },
                imgUrlB:{
                  url:'2',
                  text:' 有时候你自己就是繁琐生活中的英雄'
                },
                imgUrlC:{
                  url:'3',
                  text:' 精选 | 网络热歌分享'
                }
              },
              {
                imgUrlA:{
                  url:'8',
                  text:'徐梦圆-china系列全收录'
                },
                imgUrlB:{
                  url:'5',
                  text:' 【节奏感】那些超带感的音乐（典藏什么鬼）'
                },
                imgUrlC:{
                  url:'6',
                  text:'魔道祖师的正确打开方式^'
                }
              }
            ]
          }
      },
      computed: {
        swiper() {
          return this.$refs.mySwiper.swiper
        }
      },
      mounted() {
        // current swiper instance
        // 然后你就可以使用当前上下文内的swiper对象去做你想做的事了
        console.log('this is current swiper instance object', this.swiper)
        // this.swiper.slideTo(0, 1000, false)

      }
    }
</script>

<style scoped>
  #Discover{

    /*overflow-x: hidden;*/
    /*overflow-y: scroll;*/
    line-height: 30px;
    height: 100%;
  }
  #Discover .top {
    padding: 0 10px;
  }
  .RotationChart{
    border-radius: 5px;
  }
  /*红色圈圈按钮*/
  .optionCircular{
    margin-top: 15px;
    padding: 0 10px;
  }
  .optionCircular ul{
    display: flex;
    font-size: 10px;
  }
  .optionCircular ul li{
    flex: 1;

  }
  .optionCircular ul li:nth-of-type(5){
    width: 40px;flex: none; text-align: center;
  }
  .optionCircular ul li div{
    width: 40px;height: 40px;
    background-color:#CD3700 ;
    border-radius: 20px;
    font-size: 18px;
    color:  #F5F5F5;
    padding-top:6px;
  }
  .optionCircular ul li span{
    width: 40px;
    display: block;
    margin-top: 5px;
    text-align: center;
    color:#363636 ;
  }
  /*推荐歌单*/

  .songSheet{  /*上边框*/
    margin-top: 10px;
    padding: 0 10px;
    border-top: #E8E8E8 solid 1px;padding-top: 10px;
  }
  .songSheet ul li{
    display: flex;
    margin-top: 3px;
  }

  .songSheet ul li div{/*图片列表*/
    margin-right: 10px;
    flex: 1;
  }
  .songSheet ul li div img{
    border-radius: 4px;
  }
  .songSheet ul li div p{
    font-size: 12px;
    line-height: 14px;
    margin-top: 2px;
    height: 28px;
    display: -webkit-box;
    /* autoprefixer: off */
    -webkit-box-orient: vertical;
    /* autoprefixer: on */
    -webkit-line-clamp: 2;
    overflow: hidden;


  }
  .songSheet ul li div:nth-of-type(3){
    margin-right: 0px;
  }/*图片列表*/


  .songSheet ul li:nth-of-type(1) div:nth-of-type(1){ /*标题*/
    flex:0 1 75%;
    text-align: left;
    line-height: 25px;
  }
  .songSheet ul li:nth-of-type(1){
    margin-bottom: 3px;
  }
  .songSheet ul li:nth-of-type(1) div:nth-of-type(2){ /*标题*/
    height: 25px;
    flex:auto;
    font-size: 12px;
    border: #CFCFCF solid 1px;
    margin: 0;
    line-height: 25px;
    border-radius: 15px;
    color: #4F4F4F;
  }

  /*云村精选*/
  .cloudSelected{ /*上边框部分*/
    border-top:#E8E8E8 solid 4px ;
    margin-top: 20px;
    padding-top: 5px;
  }
  .cloudSelected .title{ /*小标题*/
    display: flex;
    padding: 0 10px;
  }
  .cloudSelected .title div{
    flex: 1;
    text-align: left;
    font-size: 12px;
  }
  .cloudSelected .title div:nth-of-type(2){/*小标题*/
    text-align: right;
  }

  .cloudSelected ul li{
    border-top: #E8E8E8 solid 5px;
    padding: 0 10px;
  }
  .cloudSelected ul li:nth-of-type(1){
    border-top: #E8E8E8 solid 1px;
  }
  .cloudSelected ul li p{
    text-align: left;
    font-size: 12px;
  }
  .cloudSelected ul li  .textImg img{
    border-radius: 8px 8px 0 0;
  }
  .cloudSelected ul li  .textImg div{
    height: 30px;
    padding-left: 10px;
    font-size: 12px;
    text-align: left;
    background-color: #E8E8E8;
    border-radius: 0 0 8px 8px;
    display: -webkit-box;
    /* autoprefixer: off */
    -webkit-box-orient: vertical;
    /* autoprefixer: on */
    -webkit-line-clamp: 1;
    overflow: hidden;
  }
  .cloudSelected ul li .operation{
   display: flex;padding-left: 5px;
  }
  .cloudSelected ul li .operation div{
    flex: 1; text-align: left;
  }
  .cloudSelected ul li .operation div:nth-of-type(1){
    width: 23%;flex: none;
  }
</style>
