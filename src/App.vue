<template>
  <div id="app">  
        <video :src="giftUri" style="width: 100px; height: 100px" type="video/mp4" id="giftVideo1"></video>
    <div class="alive">
      <!-- 礼物展示区 -->
      <div class="gift" v-show="showGiftRate">
        <video :src="giftUri" id="giftVideo"></video>
      </div>
      <!-- 礼物展示区 -->
      <!-- 舞台 -->
      <div class="stage"></div>
      <!-- 舞台 -->
      <!-- 直播视频区 -->
      <div class="video-box"> 
        <div id="video" class="video"></div>
      </div>
      <!-- 直播视频区 -->
    </div> 
    <!-- 展示条 -->
    <div class="bar"> 
        <div class="bar-item">
          <MenuGroup>
              <template slot="title"> 
                <ImgCard 
                  :src="imgs[0]"
                />
                统计分析
              </template>
              <MenuItem v-for="(uri, index) in imgs" :name="index" :key="index"> 
              {{uri}}
                <div class="img-card">
                    <img src="./assets/1.png" alt=""> 
                </div>
              </MenuItem>
          </MenuGroup> 
        </div>
        <div class="bar-item"> 
          <Button @click="handleReward">
            打赏
            <!-- 打赏下主播呗 -->
          </Button> 
        </div>  
    </div>
    <!-- 展示条 -->
    <div @click="onPlay">点我播放</div>
  </div>
</template>

<script>
  import DPlayer from 'dplayer'
  import ImgCard from './ImgCard'
  import './app.css'
  import { Button, MenuItem, MenuGroup } from 'iview' 
  const GIFTS = [
    '/static/source/1.mp4',
    '/static/source/2.mp4',
    '/static/source/3.mp4',
    '/static/source/4.mp4',
  ] 
  
  export default {
    name: "optimizeVideo",
    components: { 
      Button,
      MenuItem,
      MenuGroup,
      ImgCard,
    },
    data() {
      return {
        dp:null,
        giftUri: GIFTS[0],
        showGiftRate: true,
        imgs: [
          './../static/source/1.png',
          './../static/source/2.png',
          './../static/source/3.png',
          './../static/source/4.png',
        ]
      }
    },
    methods:{
      handleReward(){
        document.getElementById('giftVideo').src = GIFTS[Math.floor(Math.random() * 3)]
        console.log(document.getElementById('giftVideo'));

        document.getElementById('giftVideo').play() 
      },
      onPlay() {
          this.dp.play()
      }
    }, 
    created () {
      this.$nextTick(() => {
        document.getElementById('giftVideo').addEventListener('play', () => {
          this.showGiftRate = true
        })
        document.getElementById('giftVideo').addEventListener('ended', () => {
          this.showGiftRate = false
        })
      })
    },
    mounted() {
      console.log(GIFTS);
      this.dp = new DPlayer({
        // 配置参数
        container: document.getElementById('video'),
        autoplay: false,
        theme: '#FADFA3',
        loop: true,
        lang: 'zh-cn',
        preload: 'auto',
        // logo: 'logo.png',
        volume: 0.7,
        video: {
          url: './../source/live.flv',
          pic: 'dplayer.png',
          type: 'flv',
        },
      });
      
      // 禁止右键下载视频
      document.oncontextmenu = new Function("event.returnValue=false;");
      document.onselectstart = new Function("event.returnValue=false;");

      // 修改循环播放显示
      document.getElementsByClassName('dplayer-setting-item dplayer-setting-loop')[0].getElementsByClassName('dplayer-label')[0].innerText = "循环播放"
      // 修改倍速播放显示
      document.getElementsByClassName('dplayer-setting-item dplayer-setting-speed')[0].getElementsByClassName('dplayer-label')[0].innerText = "播放倍速"
    }
  }
</script>
 
