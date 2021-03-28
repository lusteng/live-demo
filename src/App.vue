<template>
  <div id="app">  
    <div class="alive">
      <!-- 礼物展示区 -->
      <div class="gift" v-show="showGiftRate">
        <p class="giftAlert">
          <em>你打赏了个</em>
          <span>{{giftWord}}</span>
          <em>给主播</em>
        </p>
        <video :src="giftUri" id="giftVideo"></video>
      </div>
      <!-- 礼物展示区 -->
      <!-- 舞台 -->
      <div class="stage" v-show="stageShow">
        <div class="">
          <div>送出</div>
        </div>
      </div>
      <!-- 舞台 -->
      <!-- 直播视频区 -->
      <div class="live-box"> 
        <div id="liveVideo" class="video"></div>
      </div>
      <!-- 直播视频区 -->
    </div> 
    <!-- 展示条 -->
    <div class="bar"> 
        <div class="bar-item" v-for="(uri, index) in imgs" :key="index">  
          <ImgCard 
            :src="uri[0]"
            :explain="uri[1]"
            @on-click="handleSentStage"
          /> 
        </div>
        <div class="bar-item"> 
          <Button @click="handleReward">
            打赏下主播呗
          </Button> 
        </div>  
    </div>
    <!-- 展示条 --> 
  </div>
</template>

<script>
  import DPlayer from 'dplayer'
  import ImgCard from './components/ImgCard'
  import './app.css'
  import { Button, } from 'iview' 
  const GIFTS = [
    require('/static/source/1.mp4'),
    require('/static/source/2.mp4'),
    require('/static/source/3.mp4'),
    require('/static/source/4.mp4'),
  ] 
  const WS = [
    '红色大跑车',
    '飞龙在天',
    '森林精灵',
    '金色麒麟',
  ]
  
  export default {
    name: "optimizeVideo",
    components: { 
      Button, 
      ImgCard, 
    },
    data() {
      return {
        dp:null,
        giftUri: GIFTS[0],
        showGiftRate: false,
        stageShow: false,
        stagImg: "",
        stagExplain: "",
        giftWord: WS[0],
        imgs: [
          [require('/static/source/1.png'), '小花花'],
          [require('/static/source/2.png'), '小皇冠'],
          [require('/static/source/3.png'), '小火箭'],
          [require('/static/source/4.png'), '小心心'],
        ]
      }
    },
    methods:{
      handleSentStage(o){ 
        console.log(src);
        let {
          src, explain
        } =  o
        this.stagExplain = explain
        this.stagImg = src
      },
      handleReward(){
        let random = Math.floor(Math.random() * 4)
        document.getElementById('giftVideo').src = GIFTS[random]
        this.giftWord = WS[random] 
        document.getElementById('giftVideo').play() 
      }, 
    },  
    mounted() { 
      this.$nextTick(() => {
        this.dp = new DPlayer({
          // 配置参数
          container: document.getElementById('liveVideo'),
          autoplay: true,
          theme: 'blue',  
          volume: 0.2,
          video: {
            url: './../live.flv', 
            type: 'flv',
          },
        });

        this.dp.play()
        // 禁止右键下载视频
        document.oncontextmenu = new Function("event.returnValue=false;");
        document.onselectstart = new Function("event.returnValue=false;");

        // 修改循环播放显示
        document.getElementsByClassName('dplayer-setting-item dplayer-setting-loop')[0].getElementsByClassName('dplayer-label')[0].innerText = "循环播放"
        // 修改倍速播放显示
        document.getElementsByClassName('dplayer-setting-item dplayer-setting-speed')[0].getElementsByClassName('dplayer-label')[0].innerText = "播放倍速"



        
        document.getElementById('giftVideo').addEventListener('play', () => {
          this.showGiftRate = true
        })
        document.getElementById('giftVideo').addEventListener('ended', () => {
          this.showGiftRate = false
        })
      })
    }
  }
</script>
 
