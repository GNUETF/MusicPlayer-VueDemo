<template>
  <div id="MusicPlayer" >
    <img :src="Music.imgurl" alt="" width="100%" height="100%">
    <audio @canplay="getDuration" @timeupdate="updateTime"  content  id="MyAudio" ref="audioClass">
      <source src="http://sg.sycdn.kuwo.cn/resource/n2/15/61/1584821976.mp3"  type="audio/mpeg">
    </audio>
    <div class="alertList" v-show="listState" @click="hideList()">
        <div class="list">
          <ul>
            <li>
              <div><i class="fa fa-refresh" aria-hidden="true"></i> 列表循环({{list.length}})</div>
              <div><i class="fa fa-plus-square-o" aria-hidden="true"></i> 收藏全部</div>
            </li>
            <li v-for="(text,index) in list" @click="Playlist(index)">{{text.name}} <span>-{{text.author}}</span></li>
          </ul>
        </div>
    </div>
    <div id="Player">
      <div class="top">
        <div @click="AjaxData"><i class="fa fa-chevron-left" aria-hidden="true"></i></div>
        <div>
          <p><b>{{Music.MyName}}</b></p>
          <p>{{Music.author}}</p>
        </div>
        <div><i class="fa fa-share" aria-hidden="true"></i></div>
      </div>
      <div class="CompactDisc">
        <img :src="Music.imgurl" alt="" width="180" height="180px">
      </div>

      <div class="div_Button">
        <ul>
          <li><i class="fa fa-heart-o" aria-hidden="true"></i></li>
          <li><i class="fa fa-download" aria-hidden="true"></i></li>
          <li><i class="fa fa-pagelines" aria-hidden="true"></i></li>
          <li><i class="fa fa-commenting-o" aria-hidden="true"></i></li>
          <li><i class="fa fa-align-center" aria-hidden="true"></i></li>
        </ul>
        <ul>
          <li>{{schedule}}</li>
          <li class="block">
            <el-slider v-model="value"></el-slider>
          </li>
          <li>{{Altogether}}</li>
        </ul>
        <ul>
          <li><i class="fa fa-exchange" aria-hidden="true"></i></li>
          <li @click="previousTrack"><i class="fa fa-step-backward" aria-hidden="true"></i></li>
          <li @click="SwitchClick">
            <div v-if="switchBtn"><i class="fa  fa-pause" aria-hidden="true"></i></div>
            <div v-else="switchBtn"><i class="fa fa-play" aria-hidden="true"></i></div>
          </li>
          <li @click="nextTrack"><i class="fa fa-step-forward" aria-hidden="true"></i></li>
          <li @click="showList"><i class="fa fa-indent" aria-hidden="true"></i></li>
        </ul>
      </div>

    </div>

  </div>

</template>

<script>
  import {MyDataMusic} from '../router/DataMusic'
    export default {
      name: "MusicPlayer",
      data(){
          return{
            //歌曲数据
            MyMusicPlayer:{},
            //当前播放
            Music:{
              MyName:'红色高跟鞋',
              author:'二珂',
              imgurl:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1555499832246&di=3d4fe30c844eabb99fb505dcb8bfc531&imgtype=0&src=http%3A%2F%2Fphoto.tuchong.com%2F1697143%2Ff%2F24148720.jpg',
              src:'http://sg.sycdn.kuwo.cn/resource/n2/15/61/1584821976.mp3',
              Lyric:'暂无歌词',
              State:false
            },
            //进度
            schedule:0,
            //进度条
            value:0,
            //歌曲时长
            Altogether:0,
            TotalMin:0,

            // 播放图标
            switchBtn:false,
            num:0,//第几曲

            //播放列表
            list:[],
            listState:false
          }
      },
      props:{
        nub:{
          type:Number,
          default:function () {
            return 1
          }
        }
      },
      mounted() {
        this.MyMusicPlayer=MyDataMusic()
        this.list=this.MyMusicPlayer.RecentlyPlay

      },
      methods:{
        // 获取当前歌曲总时长
        getDuration() {
          this.TotalMin=this.$refs.audioClass.duration; //此时可以获取到duration

          this.Altogether = (Math.ceil(this.TotalMin%3600/60)-1)>10 ? (Math.ceil(this.TotalMin%3600/60)-1) : '0'+(Math.ceil(this.TotalMin%3600/60)-1)+':'+(Math.ceil(this.TotalMin%3600%60)>10?Math.ceil(this.TotalMin%3600%60):'0'+Math.ceil(this.TotalMin%3600%60));
          // this.schedule=Math.ceil(this.$refs.audioClass.currentTime)
        },
        // 获取当前播放进度时长和进度条值处理
        updateTime(e) {

          let min = Math.ceil( e.target.currentTime );  //获取audio当前播放时间
          this.value = ( min/((this.TotalMin/60)*60) )*100;
          this.schedule = (Math.ceil(min%3600/60)-1)>10 ? (Math.ceil(min%3600/60)-1) : '0'+(Math.ceil(min%3600/60)-1)+':'+(Math.ceil(min%3600%60)>10?Math.ceil(min%3600%60):'0'+Math.ceil(min%3600%60))
          2<this.value<99?this.switchBtn=true:this.switchBtn=false;//图标切换
          console.log(this.value);
          if(Math.round(this.value)==100){//自动播放下一首
            this.$refs.audioClass.pause();
            this.nextTrack()
            console.log('下一首')
          }
        },
        //播放关闭歌曲
        SwitchClick(){
          this.switchBtn=!this.switchBtn
          if(this.switchBtn){
            this.$refs.audioClass.play()
          }else{
            this.$refs.audioClass.pause()
          }
        },
        //上一曲
        previousTrack(){
          if (this.num-1<0){
            this.num=this.MyMusicPlayer.RecentlyPlay.length-1
          } else {
            this.num=this.num-1
          }

          // this.MyMusicPlayer.RecentlyPlay[0].src
          this.Music.src=this.MyMusicPlayer.RecentlyPlay[this.num].src;
          this.Music.MyName=this.MyMusicPlayer.RecentlyPlay[this.num].name;
          this.Music.author=this.MyMusicPlayer.RecentlyPlay[this.num].author;
          this.Music.imgurl=this.MyMusicPlayer.RecentlyPlay[this.num].imgurl;
          console.log(this.Music.imgurl)
          if(this.Music.imgurl==''){
            this.Music.imgurl=require('../assets/back.jpg')
            console.log('45656')
          }
          this.$refs.audioClass.src=this.MyMusicPlayer.RecentlyPlay[this.num].src;
          this.$refs.audioClass.play();
          this.switchBtn=true;
        },
        //下一曲
        nextTrack(){
          if (this.num+1>this.MyMusicPlayer.RecentlyPlay.length-1){
            this.num=0
          } else {
            this.num=this.num+1
          }
          // this.MyMusicPlayer.RecentlyPlay[0].src
          this.Music.src=this.MyMusicPlayer.RecentlyPlay[this.num].src;
          this.Music.MyName=this.MyMusicPlayer.RecentlyPlay[this.num].name;
          this.Music.author=this.MyMusicPlayer.RecentlyPlay[this.num].author;
          this.Music.imgurl=this.MyMusicPlayer.RecentlyPlay[this.num].imgurl;
          console.log(this.Music.imgurl)
          if(this.Music.imgurl==''){
            this.Music.imgurl=require('../assets/back.jpg')
            console.log('45656')
          }
          this.$refs.audioClass.src=this.MyMusicPlayer.RecentlyPlay[this.num].src;
          this.$refs.audioClass.play()
          this.switchBtn=true;

        },
        //播放列表
        Playlist(index){  //切歌
          console.log(index)
          this.Music.src=this.MyMusicPlayer.RecentlyPlay[index].src;
          this.Music.MyName=this.MyMusicPlayer.RecentlyPlay[index].name;
          this.Music.author=this.MyMusicPlayer.RecentlyPlay[index].author;
          this.Music.imgurl=this.MyMusicPlayer.RecentlyPlay[index].imgurl;
          console.log(this.Music.imgurl)
          if(this.Music.imgurl==''){
            this.Music.imgurl=require('../assets/back.jpg')
            console.log('45656')
          }
          this.$refs.audioClass.src=this.MyMusicPlayer.RecentlyPlay[index].src;
          this.$refs.audioClass.play()
          this.switchBtn=true;
          this.hideList();
          this.num=index;
        },
        //显示列表
        showList(){
          this.listState=true
        },
        //隐藏列表
        hideList(){
          this.listState=false
        },
        AjaxData(){
          this.$emit("MusicPlayerData",this.Music)
        }
      },
      watch:{
        value:function (val) {
          this.$emit("valueData", val)
        }
      }
    }
</script>

<style scoped>
  #MusicPlayer{
    width: 100%;height: 100%;
  }
  #Player{
    width: 100%;height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background:rgba(0,0,0,0.8) ;
  }

  .top{
    display: flex;
    height: 45px;
  }
  .top div{
    flex: none; width: 60px;
    line-height: 45px;
    font-size: 20px;
    text-align: left;
    padding-left: 10px;
    color: #e8e8e8;
  }
  .top div:nth-of-type(2){
    flex: 1;
    font-size: 14px;
    line-height:20px;
    text-align: center;
    padding-top:5px ;

  }
  .top div:nth-of-type(2) p:nth-of-type(2){
    font-size: 12px;
    line-height:20px;
    color: #CDBA96;
  }
  .top div:nth-of-type(3){
    text-align: right;
    padding: 0 10px 0 0px;
  }

  .CompactDisc{
    width: 100%;
    height: 50%;
    padding-top: 30%;
  }
  .CompactDisc img{
    border-radius: 50%;
    border: #909399 solid 4px;
  }
  .div_Button{
    width: 100%; height:auto;
    position: absolute;
    /*z-index: 100;*/
    bottom: 0;
    left: 0;
  }
  .div_Button ul{
    display: flex;
  }
  .div_Button ul li{
    flex:1;
    color: #e8e8e8;
    font-size: 20px;
  }
  .div_Button ul:nth-of-type(2) li{
    font-size: 12px;line-height: 38px;
  }
  .div_Button ul:nth-of-type(2) li{
    flex:none;width:50px
  }
  .div_Button ul:nth-of-type(2) li:nth-of-type(2){
    flex: 1;
  }
  .div_Button ul:nth-of-type(3){

   line-height: 50px;padding-bottom: 15px;

  }
  .div_Button ul:nth-of-type(3) li:nth-of-type(3){
    flex: none; width: 50px;
  }
  .div_Button ul:nth-of-type(3) li:nth-of-type(3) div{

    width: 50px; height: 50px;
    border: #e8e8e8 solid 2px;
    border-radius: 50%;
  }
  /*歌曲列表*/
  .alertList{
    position: absolute;
    top: 0;left: 0;
    z-index: 1002;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.8);

  }
  .alertList .list{
    width: 100%;
    height:60%;
    background: #f5f5f5;
    position: absolute;
    bottom: 0;
    left: 0;
    border-radius: 15px 15px 0 0;
    padding-top: 35px;

  }
  .alertList .list ul{
    height: 100%;
    overflow-x: hidden;
    overflow-y:  auto;
    text-align: center;
  }
  .alertList .list ul li:nth-of-type(1){
    width: 100%;
    position: absolute;
    top: 5px;
    display: flex;
    text-align: left;
    border-bottom: #e8e8e8 solid 1px;
  }
  .alertList .list ul li:nth-of-type(1) div{
    width: 50%;
  }
  .alertList .list ul li{
    text-align: left;
    padding: 0 12px;
    height: 30px;
    line-height: 30px;
  }
  .alertList .list ul li span{
    font-size: 10px;
    color: #909399;
  }

  .alertList .list ul li:nth-of-type(1) div:nth-of-type(2){
    text-align: right;
  }

</style>
