<template>
  <div>

    <header>
      <ul>
        <li><i class="fa fa-bars" aria-hidden="true"></i></li>
        <li>我的</li>
        <li>发现</li>
        <li>朋友</li>
        <li>视频</li>
        <li><i class="fa fa-search" aria-hidden="true"></i></li>
      </ul>
    </header>
    <section :style="{height:HEIGHT}">
      <swiper >
        <swiper-slide :style="{height:HEIGHT}" class="section_slide">
          <MyMusic></MyMusic>
          <!--<Discover></Discover>-->
        </swiper-slide>
        <swiper-slide :style="{height:HEIGHT}" class="section_slide">
          <Discover></Discover>
        </swiper-slide>
      </swiper>
    </section>
    <footer>
      <ul>
        <li @click="MusicState">
          <img :src="Music.imgurl" alt="" width="35" height="35">
        </li>
        <li  @click="MusicState">
          <div>{{Music.MyName}}</div>
          <div>{{Music.author}}</div>
        </li>
        <li @click="Playback">
          <el-progress type="circle" :percentage="percentage" status="text"  :width="30" :stroke-width="2" color="#EE4000">
              <i v-if="PlaybackStatus" class="fa fa-pause" aria-hidden="true"></i>
              <i v-else class="fa fa-play" aria-hidden="true"></i>
          </el-progress>
        </li>
        <li><i class="fa fa-outdent" aria-hidden="true"></i></li>
      </ul>
    </footer>
    <!--弹出层-->
    <div id="PlayAlert" v-show="PlayerState">
      <MusicPlayer @MusicPlayerData="MusicPlayerData" @valueData="MyVal" ></MusicPlayer>
    </div>
  </div>
</template>

<script>
  import 'font-awesome/css/font-awesome.min.css'

  import Discover from '../components/Discover'
  import MyMusic from '../components/MyMusic'
  import MusicPlayer from '../components/MusicPlayer'
  export default {
        name: "Home",
      data(){
        return{
          sectionOption: {
            pagination: {
            },
            autoplay:true,
          },
          Music:{
            MyName:'红色高跟鞋',
            author:'二珂',
            imgurl:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1555499832246&di=3d4fe30c844eabb99fb505dcb8bfc531&imgtype=0&src=http%3A%2F%2Fphoto.tuchong.com%2F1697143%2Ff%2F24148720.jpg',
            src:'http://sg.sycdn.kuwo.cn/resource/n2/15/61/1584821976.mp3',
            Lyric:'暂无歌词',
            State:false,
          },
          MyAudio:{},//播放器对象
          PlaybackStatus:false,//是否播放音乐footer 图标
          PlayerState:false,//显示播放详情页面
          HEIGHT:"",
          percentage:0


        }
      },
    mounted() {
          this.HEIGHT=(window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight)-90+"px"

    },
    methods:{
      Playback(){ //是否播放歌曲
        this.PlaybackStatus=!this.PlaybackStatus
         this.MyAudio= document.getElementById("MyAudio")
        if(this.PlaybackStatus){
          MyAudio.play()
        }else{
          MyAudio.pause()
        }
      },
      //打开播放页
      MusicState(){
        this.PlayerState=true
        console.log(this.PlayerState)
      },
      MusicPlayerData(data){
        this.Music=data;
        this.PlayerState=data.State
      },
      MyVal(val){
        2<val<99?this.PlaybackStatus=true:this.PlaybackStatus=false;
        this.percentage=val
      },
    },
    components:{
      Discover,
      MyMusic,
      MusicPlayer
    }
  }
</script>

<style scoped>
  header{
    height: 45px;width: 100%;
    background-color:#F5F5F5 ;
    /*position:fixed;*/
    /*z-index: 2;*/
    /*top: 0;*/
    font-size: 16px;
    line-height: 40px;

  }
  header ul{display: flex; margin-top: 2px}
  header ul li{
    flex: 1;
  }
  header ul li:nth-of-type(1){
    text-align: left;padding-left: 10px;

  }
  header ul li:nth-of-type(6){
    text-align:right;padding-right: 10px;

  }

  .section_slide{
    /*margin-top: 50px;*/
    /*margin-bottom: 45px;*/
    overflow-x: hidden;
    overflow-y:  auto;

    text-align: center;
  }


  footer{
    height: 45px;width: 100%;
    position:fixed;
    z-index: 2;
    bottom: 0;
    border-top: #E8E8E8 solid 1px;
    background-color:#F5F5F5 ;
  }
  footer ul{
    display: flex;
    margin-top: 2px;
  }
  footer ul li{
    width: 40px;
    flex: none;
  }
  footer ul li:nth-of-type(2){
    flex: 1;
    padding-top: 5px;
    text-align: left;
    font-size: 12px;
  }
  footer ul li:nth-of-type(1) img{
    border-radius: 50%;
    margin-top: 2px;
    margin-left: 10px;
  }
  footer ul li:nth-of-type(2) div:nth-of-type(1){
    padding-left: 15px;
  }
  footer ul li:nth-of-type(2) div:nth-of-type(2){
    -webkit-transform: scale(0.85)
  }
  footer ul li:nth-of-type(4){
    margin-top: 10px;
    font-size: 20px;
    color: #4F4F4F;
  }
  footer ul li:nth-of-type(3){
    padding-top: 5px;
  }

  #PlayAlert{
    width: 100%;
    height: 100%;
    position: absolute;
    z-index: 99;
    top: 0;
    left: 0;

  }
</style>
