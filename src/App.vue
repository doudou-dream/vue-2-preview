<template>
  <div id="app">
    <el-button @click="wayImg">图片</el-button>
    <el-button @click="wayPlayer">视频</el-button>
    <el-button @click="wayAudioList">音频</el-button>
    <el-button @click="wayPdfUrl">pdf</el-button>
    <Preview ref="preview" :item="dataPreview"/>
  </div>
</template>

<script>
import Preview from './components/Preview.vue'
import falling from './assets/falling-star.mp3'
import img from './assets/95890a3542124e3aaf7c6fa2f99af45d.jpg'

export default {
  name: 'App',
  components: {
    Preview,
  },
  data() {
    return {
      dataPreview: {
        show: false,
        title: '',
        url: '',
        type: ''
      },
    }
  },
  methods: {
    wayImg() {
      this.dataPreview.show  = true
      this.dataPreview.title = '图片'
      this.dataPreview.url   = img//'https://bqb12.bingping.top/Uploads/vod/2019-02-27/5c7562cea3428.jpg'
    },
    wayPlayer() {
      this.dataPreview.show  = true
      this.dataPreview.title = 'MP4'
      this.dataPreview.url   = '//player.alicdn.com/video/aliyunmedia.mp4'
    },
    wayPdfUrl() {
      this.dataPreview.show  = true
      this.dataPreview.title = 'pdf'
      this.dataPreview.url   = './demo.pdf'
    },
    wayAudioList() {
      this.dataPreview.show  = true
      this.dataPreview.title = 'mp3'
      this.dataPreview.url   = falling
    }
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 60px;
}
</style>
