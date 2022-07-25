<script>
import pdf from 'vue-pdf'
import AudioPlayer from '@liripeng/vue-audio-player'
import VueAliplayerV2 from 'vue-aliplayer-v2'

export default {
  name: 'HelloWorld',
  props: {
    item: {
      type: Object,
      default: () => {
        return {
          show: false,//是否展示
          title: '',//标题
          url: '',// 地址
          cover: '',//视频封面图
          type: ''// img,mp4,mp3,pdf
        }
      }
    }
  },
  components: {
    pdf,
    AudioPlayer,
    VueAliplayerV2
  },
  data() {
    return {
      //img 图片
      imgUrl: '',
      //pdf
      audioList: [
        {
          name: '',
          url: '',
        },
      ],
      // 视频
      aliplayer: {
        source: '',
        options: {
          autoplay: false,
          cover: ''
        }
      },
      // pdf地址
      pdf: {
        arrPages: [],
        src: null
      }
    }
  },
  watch: {
    'item.show': {
      handler: function (boo) {
        if (!boo) {
          return false
        }
        if (!this.item.type) {
          this.item.type = this.getFormat(this.item.url)
        }
        // 数据初始化
        switch (this.item.type) {
          case 'img':
            this.imgUrl = this.item.url
            break
          case 'mp3':
            this.audioList[0].url = this.item.url
            break
          case 'mp4':
            this.aliplayer.source        = this.item.url
            this.aliplayer.options.cover = this.item.cover
            break
          case 'pdf':
            this.pdf.src = pdf.createLoadingTask(this.item.url) || {}
            this.pdf.src.promise.then(pdf => {
              for (let i = 1; pdf.numPages >= i; i++) {
                this.pdf.arrPages.push(i)
              }
            })
            break
        }
      }
    }
  },
  render() {
    let temp = <el-empty description="空"></el-empty>
    switch (this.item.type) {
      case'img':
        temp =
            <div style="display: flex;flex-direction: column;justify-content: center;align-items: center;">
              <el-image src={this.imgUrl}>
                <template slot="placeholder">
                  <div class="placeholder">
                    <i class="el-icon-loading"/>
                    <p>加载中</p>
                  </div>
                </template>
                <template slot="error">
                  <div class="placeholder">
                    <p>加载失败</p>
                  </div>
                </template>
              </el-image>
            </div>
        break
      case'mp4':
        temp = <vue-aliplayer-v2 options={this.aliplayer.options} source={this.aliplayer.source}/>
        break
      case'mp3':
        temp =
            <div style="padding: 88px 60px;">
              <audio-player
                  ref="audioPlayer"
                  show-prev-button={false}
                  show-next-button={false}
                  audio-list={this.audioList.map(elm => elm.url)}
                  theme-color="#004daa"
              />
            </div>
        break
      case'pdf':
        temp =
            <div style="overflow:auto;display: block;height: 100%">
              {
                this.pdf.arrPages.map(val => {
                  return <pdf key={val} page={val} src={this.pdf.src}/>
                })
              }
            </div>
        break
    }
    return (
        <el-dialog
            v-on:close={this.wayClose}
            title={this.item.title}
            visible={this.item.show}
            {...{on: {'update:visible': val => this.item.show = val}}}
            append-to-body
            width="80%">
          {temp}
        </el-dialog>
    )
  },
  methods: {
    // 输入文件地址','返回文件格式
    getFormat(value) {
      let arr = value.match(/^.*\.(.{3,4})$/)
      if (arr.length < 2) {
        return ''
      }
      let suffix = arr[1].toUpperCase()
      let img    = ['BMP', 'JPG', 'PNG', 'TIF', 'GIF', 'PCX', 'TGA', 'EXIF', 'FPX', 'SVG', 'PSD', 'CDR', 'PCD', 'DXF', 'UFO', 'EPS', 'AI', 'RAW', 'WMF', 'WEBP', 'AVIF', 'APNG']
      if (img.some(val => val === suffix)) {
        return 'img'
      }
      let mp3 = ['MP3', 'WMA', 'WAV', 'APE', 'FLAC', 'OGG', 'AAC']
      if (mp3.some(val => val === suffix)) {
        return 'mp3'
      }
      let mp4 = ['AVI', 'WMV', 'MPEG', 'MP4', 'M4V', 'MOV', 'ASF', 'FLV', 'F4V', 'RMVB', 'RM', '3GP', 'VOB']
      if (mp4.some(val => val === suffix)) {
        return 'mp4'
      }
      if (suffix === 'PDF') {
        return 'pdf'
      }
      return ''
    },
    // 弹窗关闭
    wayClose() {
      //数据清空
      // 类型
      this.item.type               = ''
      // pdf
      this.pdf.src                 = null
      this.pdf.arrPages            = []
      // 图片
      this.imgUrl                  = ''
      // 音频
      this.audioList[0].url        = ''
      // 视频
      this.aliplayer.source        = ''
      this.aliplayer.options.cover = ''
    }
  }
}
</script>

<style lang="less" scoped>
.placeholder {
  height: 5rem;
  width: 5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
