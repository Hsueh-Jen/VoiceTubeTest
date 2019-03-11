<template>
  <div class="body">
    <ul class="navigation">
   <div class="label">排序</div>
   <li v-bind:class="{ active: sortElement=='publish' }">
      <a v-on:click="sortBy('publish')"><span>發布時間</span></a>
   </li>
   <li v-bind:class="{ active: sortElement=='views' }">
      <a v-on:click="sortBy('views')"><span>觀看次數</span></a>
   </li>
   <li v-bind:class="{ active: sortElement=='collectCount' }">
      <a v-on:click="sortBy('collectCount')"><span>收藏次數</span></a>
   </li>
</ul>

    <ul class="navigation">
   <div class="label">長度</div>
   <li v-bind:class="{ active: filterElement[0]==0&&filterElement[1]==-1 }">
      <a v-on:click="filterByVideoLength(0,-1)"><span>不限</span></a>
   </li>
   <li v-bind:class="{ active: filterElement[0]==0&&filterElement[1]==4 }">
      <a v-on:click="filterByVideoLength(0,4)"><span>4分鐘以下</span></a>
   </li>
   <li v-bind:class="{ active: filterElement[0]==5&&filterElement[1]==10 }">
      <a v-on:click="filterByVideoLength(5,10)"><span>5-10分鐘</span></a>
   </li>
   <li v-bind:class="{ active: filterElement[0]==10&&filterElement[1]==-1 }">
      <a v-on:click="filterByVideoLength(10,-1)"><span>超過10分鐘</span></a>
   </li>
</ul>
<hr>

  <div class="videos">
      <video-div  v-for="(video) in videos"
        :imgSrc="video.thumbnail"
        :title="video.title"
        :views="video.views"
        :duration="video.duration"
        :level="video.level"
        :captions="video.captions"
      />
    </div>

  </div>

</template>

<script>
import axios from 'axios'
import VideoDiv from './VideoDiv'
export default {
  name: 'MainPage',
  methods:{
    async getVideoInfos (url, data) {
    try {
      let res = await axios.get('https://us-central1-lithe-window-713.cloudfunctions.net/fronted-demo')
      return new Promise((resolve) => {
        if (res.status === 200) {
          this.result.data= this.videos =res.data.data;
          resolve(res);
        } else {
          resolve(res)
        }
      })
    } catch (err) {
      alert('There is a error from server.')
      console.log(err)
    }
  },
  sortBy(rule){
    this._data.sortElement=rule;
    let videos = this._data.videos;
    function compare(a, b) {
      if (a[rule]>b[rule]) {
        return -1;
      }
      if (a[rule]<b[rule]) {
        return 1;
      }
      return 0;
    }
    videos.sort(compare);
    this._data.videos=videos;
  },
  filterByVideoLength(startTime,endTime){
    this._data.filterElement=[startTime,endTime];
    let videos = this._data.result.data;
    videos = videos.filter(v => {
      if(endTime==-1){ // -1表示到無上限的意思
        return v.duration > startTime*60;
      }else{
        return v.duration > startTime*60 && v.duration < endTime*60;
      }
    });
    this._data.videos=videos;
  }
  },
  beforeMount(){
    this.getVideoInfos();
  },
  data () {
    return {
      sortElement:'publish',
      filterElement:[0,-1]
      ,result:{},
      videos:[]
    }
  },
  components: {VideoDiv}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

.navigation li{
  font-family: NotoSansCJKtc-Regular;
  font-size: 12px;
  color: #787878;
  letter-spacing: 0;
  line-height: 16px;
  cursor:pointer;
}

.navigation li.active{
  background:#4283E4;
  border-radius:4px;
  color: #FFFFFF;
  padding:5px;
}

.navigation .label{
  display:inline-block;
  font-weight:900;
}

.navigation{
  display:inline-block;
}

.body{
 max-width:1366px;
}

.videos{
  display: flex;
  flex-wrap: wrap;
}
</style>
