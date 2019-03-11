<template>
  <div class="video">
    <div class="thumbnail">
      <div class="img" :style="'background: url('+imgSrc+') center center no-repeat;'"></div>
      <div class="time">{{parseInt(duration/60)}}:{{duration%60<10?'0':''}}{{duration%60}}</div>
    </div>
    <div class="textarea">
      <div class="title">{{title}}</div>
      <div class="viewarea">
        <Headphones />
        {{views}}
    </div>
      <div style="text-align:right;">
      <div v-if="level==1" class="badage green">初級</div>
      <div v-if="level==2" class="badage blue">中級</div>
      <div v-if="level==3" class="badage orange">中高級</div>
      <div v-if="level==4" class="badage black">高級</div>

      <div v-for="(caption) in captions" class="badage">
      <span v-if="caption=='cht'">中文</span>
      <span v-if="caption=='ja'">日文</span>
      <span v-if="caption=='vi'">越南文</span>
      <span v-if="caption=='en'">英文</span>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
import Headphones from "vue-material-design-icons/Headphones.vue"

export default {
  name: 'VideoDiv',
  props:['imgSrc','title','views','duration','level','captions'],
  data () {
    return {
    }
  },
  components:{Headphones}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.img{
  width:300px;
  height:220px;
  cursor:pointer;
  transition: all .2s ease-in-out;
}

.img:hover,
.img:focus{
  -ms-transform: scale(1.2);
  -moz-transform: scale(1.2);
  -webkit-transform: scale(1.2);
  -o-transform: scale(1.2);
  transform: scale(1.2);
}

.video{
  width:300px;
  float:left;
  margin:3px;
  background: #FFFFFF;
  box-shadow: 0 1px 3px 0 rgba(0,0,0,0.30);
  border-radius: 4px;
}
.title{
  font-family: NotoSansCJKtc-Regular;
  color: #212B36;
  cursor:pointer;
  margin-bottom:5px;
  font-size: 1em;
  line-height: 1.2em;
  height: 2.4em;
  overflow: hidden;
}

.thumbnail{
  width:100%;
  position:relative;
  overflow:hidden;
}

.badage {
  background: #4283E4;
  border-radius: 2px;
  display:inline-block;
  font-family: PingFangTC-Medium;
  font-size: 12px;
  color: #FFFFFF;
  letter-spacing: -0.03px;
  padding:5px;
  margin-right:5px;
}

.viewarea{
  padding-bottom:5px;
  font-family: material;
  font-size: 19px;
  color: #787878;
}

.time{
  color:white;
  background: rgba(0,0,0,0.80);
  position: absolute;
  bottom:10px;
  right:10px;
  border-radius:2px;
  padding:2px 5px;
}

.textarea{
  padding:0 10px 10px 10px;
}

.green{
  background-color:#468847;
}
.blue{
  background-color:#3a87ad;
}

.orange{
  background-color:#F59B00;
}

.black{
  background-color:#333;
}
</style>
