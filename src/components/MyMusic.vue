<template>
    <div id="MyMusic">
      <div class="top">
        <swiper :options='swiperMusic'>
          <swiper-slide  v-for="(icon,index) in Circular" :key="index" class="topMusic">
            <div class="optionCircular">
              <ul>
                <li>
                  <div> <i :class="icon.icon" aria-hidden="true"></i> </div>
                  <span>{{icon.title}}</span>
                </li>
              </ul>
            </div>
          </swiper-slide>
        </swiper>
      </div>
      <div class="MyList">
        <ul>
          <li>
            <div><i class="fa fa-music" aria-hidden="true"></i></div>
            <div> 本地音乐 ( 0 ) </div>
          </li>
          <li>
            <div><i class="fa fa-play-circle-o" aria-hidden="true"></i></div>
            <div>最近播放 ( {{(MyMusicData.RecentlyPlay).length}} ) </div>
          </li>
          <li>
            <div><i class="fa fa-cloud-download" aria-hidden="true"></i></div>
            <div>下载管理 ( 0 ) </div>
          </li>
          <li>
            <div><i class="fa fa-assistive-listening-systems" aria-hidden="true"></i></div>
            <div>我的电台 ( 1 ) </div>
          </li>
          <li>
            <div><i class="fa fa-user-plus" aria-hidden="true"></i></div>
            <div>我的收藏 ( 3 ) </div>
          </li>

        </ul>
      </div>
      <div class="EstablishSongSheet">
        <ul>
          <li>
            <div><i class="fa fa-angle-down" aria-hidden="true"></i><b> 创建的歌单</b> (3)</div>
            <div><i class="fa fa-plus" aria-hidden="true"></i><span><i class="fa fa-ellipsis-v" aria-hidden="true"></i></span></div>
          </li>
          <li>
            <div :style="{backgroundImage:'url('+require('../assets/8.jpg')+')'}">
              <div><i class="fa fa-heart-o" aria-hidden="true"></i></div>
            </div>
            <div>
              <p>我喜欢的音乐</p>
              <p>{{(MyMusicData.ILike).length}}首</p>
            </div>
            <div>
              <span><i class="fa fa-heartbeat" aria-hidden="true"></i> 心动模式</span>
            </div>
          </li>
          <li v-for="list in EstablishSongSheet">
            <div :style="{backgroundImage:'url(' + list.url + ')'}"></div>
            <div>
              <p>{{list.title}}</p>
              <p>{{list.num}}首 </p>
            </div>
            <div><i class="fa fa-ellipsis-v" aria-hidden="true"></i></div>
          </li>
        </ul>
        <ul>
          <li>
            <div><i class="fa fa-angle-down" aria-hidden="true"></i><b>收藏的的歌单</b> (3)</div>
            <div><span><i class="fa fa-ellipsis-v" aria-hidden="true"></i></span></div>
          </li>
          <li v-for="list in CollectionSongSheet">
            <div :style="{backgroundImage:'url(' + list.url + ')'}"></div>
            <div>
              <p>{{list.title}}</p>
              <p>{{list.num}}首 </p>
            </div>
            <div><i class="fa fa-ellipsis-v" aria-hidden="true"></i></div>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
  import {MyDataMusic} from '../router/DataMusic'
    export default {
        name: "MyMusic",
      data(){
          return{
            swiperMusic: {
              slidesPerView: 5.5,
              spaceBetween:0,
            },
            //一排球
            Circular:[
              {
                icon:"fa fa-assistive-listening-systems",
                title:"私人FM"
              },
              {
                icon:"fa fa-braille",
                title:"最新电音"
              },
              {
                icon:"fa fa-eercast",
                title:"Sati空间"
              },
              {
                icon:"fa fa fa-heart",
                title:"私藏推荐"
              },
              {
                icon:"fa fa-user-secret",
                title:"爵士电台"
              },
              {
                icon:"fa fa-camera-retro",
                title:"古典专区"
              },
              {
                icon:"fa fa-users",
                title:"亲子频道"
              },
              {
                icon:"fa fa-car",
                title:"驾驶模式"
              }
            ],
            //创建的歌单
            EstablishSongSheet:[
              {
                url:require("../assets/Establish1.jpg"),
                title:"中国风",
                num:59
              },
              {
                url:require("../assets/Establish2.jpg"),
                title:"动漫",
                num:21
              }
            ],
            //收藏的歌单
            CollectionSongSheet:[
              {
                url:require("../assets/Collection1.jpg"),
                title:"民谣",
                num:59
              },
              {
                url:require("../assets/Collection2.jpg"),
                title:"国风民谣-满堂花醉三千客, 一剑霜寒十四州",
                num:21
              }
            ],
           //歌曲数据
            MyMusicData:{}
          }
      },
      mounted() {
          this.MyMusicData=MyDataMusic()
        //创建的歌单
          this.EstablishSongSheet[0].num=MyDataMusic().ChineseStyle.length;//中国风
          this.EstablishSongSheet[1].num=MyDataMusic().Comic.length;//动漫
        //收藏的歌单
          this.CollectionSongSheet[0].num=MyDataMusic().Ballad.length;//民谣
          this.CollectionSongSheet[1].num=MyDataMusic().CountryBallad.length//国风民谣

      }
    }
</script>

<style scoped>

   .MyList{
     border-top:#E8E8E8 solid 1px ;
     margin-top: 15px;
     padding:0  10px;
   }
   .EstablishSongSheet{
    padding:0  10px;
     border-top: #e8e8e8 solid 5px;
     padding-bottom: 10px;
  }
  /*红色圈圈按钮*/
   .topMusic:nth-of-type(1){
     margin-left: 10px;
   }
  .optionCircular{
    margin-top: 10px;
  }
  .optionCircular ul{
    display: flex;
    font-size: 10px;
  }
  .optionCircular ul li{
    flex: 1;
  }
  .optionCircular ul li div{
    width: 40px;height: 40px;
    background-color:#FF6A6A ;
    border-radius: 20px;
    font-size: 18px;
    color:  #F5F5F5;
    padding-top:6px;
    line-height: 30px;
  }
  .optionCircular ul li span{
    width: 40px;
    display: block;
    margin-top: 5px;
    text-align: center;
    color:#363636 ;
  }
  /*列表*/
  .MyList ul{
    padding-bottom: 5px;
  }
  .MyList ul li{
    display: flex;
    margin-top: 10px;

  }
  .MyList ul li div{
    flex: 1;
    border-bottom: #E8E8E8 solid 1px;
    text-align: left;
  }
   .MyList ul li:nth-of-type(5) div{
     border: 0;
   }
  .MyList ul li div:nth-of-type(1){
    flex:none;width: 17%;
    padding-left: 8px;
    border:0;
    font-size: 23px;
    /*text-align: left;*/
  }
  /*歌单*/

  .EstablishSongSheet ul li{
    display: flex;
    margin-top: 10px;
  }
   .EstablishSongSheet ul li div{
     flex:1;text-align: left;
   }
   .EstablishSongSheet ul li div:nth-of-type(2){
    text-align: right;
  }
   /*li2*/
   .EstablishSongSheet ul li:nth-of-type(2) div:nth-of-type(1){
     flex: none;
     width: 50px;height: 50px;
     background-repeat: no-repeat;
     background-size: 100% 100%;
     -moz-background-size: 100% 100%;
     border-radius: 4px;
   }
   .EstablishSongSheet ul li:nth-of-type(2) div:nth-of-type(1) div{
     background: rgba(0,0,0,0.7);
     color: white;
     font-size: 30px;
     text-align: center;
     line-height: 50px;
   }
   .EstablishSongSheet ul li div:nth-of-type(2) p{
     text-align: left;
     padding-left: 10px;
     line-height: 25px;
     display: -webkit-box;
     /* autoprefixer: off */
     -webkit-box-orient: vertical;
     /* autoprefixer: on */
     -webkit-line-clamp: 1;
     overflow: hidden;
   }

   .EstablishSongSheet ul li div:nth-of-type(2) p:nth-of-type(2){
     font-size: 12px;
   }
   .EstablishSongSheet ul li:nth-of-type(2) div:nth-of-type(3){
     text-align: right;
   }
   .EstablishSongSheet ul li:nth-of-type(2) div:nth-of-type(3) span{
      line-height: 50px;
     border: #82848a solid 1px;
     border-radius: 10px;
     padding:2px  8px;
     font-size: 12px;
   }
   /*li3*/
   .EstablishSongSheet ul li div:nth-of-type(1){
     flex: none;
     width: 50px;height: 50px;
     background-repeat: no-repeat;
     background-size: 100% 100%;
     -moz-background-size: 100% 100%;
     border-radius: 4px;
   }
   .EstablishSongSheet ul li div:nth-of-type(3){
     text-align: right;
     padding-right: 5px;
     color:#909399;
     line-height: 50px;
   }
  /*li1*/
   .EstablishSongSheet ul li:nth-of-type(1) div{
     flex:1;text-align: left;height: auto;
   }
   .EstablishSongSheet ul li:nth-of-type(1) div:nth-of-type(2){
     text-align: right;
   }
   .EstablishSongSheet ul li:nth-of-type(1) div:nth-of-type(2) span{
     padding:0 5px 0 15px;
   }

</style>
