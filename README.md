# 文件预览
> - 支持预览类型有：视频，音频，pdf，img
> - 依赖与 less，element-ui，vue-aliplayer-v2，vue-pdf，
> @liripeng/vue-audio-player 以及vue2版本

## 实例
```vue
<template>
  <div id="app">
    <el-button @click="wayImg">图片</el-button>
    <el-button @click="wayPlayer">视频</el-button>
    <el-button @click="wayAudioList">音频</el-button>
    <el-button @click="wayPdfUrl">pdf</el-button>
    <Preview :item="datas"/>
  </div>
</template>

<script>
import Preview from './components/Preview.vue'
import falling from './assets/falling-star.mp3'

export default {
  name: 'App',
  components: {
    Preview
  },
  data() {
    return {
      datas: {
        show: false,
        title: '',
        url: '',
        type: ''
      }
    }
  },
  methods: {
    wayImg() {
      this.datas.show = true
      this.datas.title = '图片'
      this.datas.type = 'img'
      this.datas.url  = 'https://bqb12.bingping.top/Uploads/vod/2019-02-27/5c7562cea3428.jpg'
    },
    wayPlayer() {
      this.datas.show  = true
      this.datas.title = 'MP4'
      this.datas.type  = 'mp4'
      this.datas.url   = '//player.alicdn.com/video/aliyunmedia.mp4'
    },
    wayPdfUrl() {
      this.datas.show  = true
      this.datas.title = 'pdf'
      this.datas.type  = 'pdf'
      this.datas.url   = './demo.pdf'
    },
    wayAudioList() {
      this.datas.show  = true
      this.datas.title = 'mp3'
      this.datas.type  = 'mp3'
      this.datas.url   = falling
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

```
## 运行
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

