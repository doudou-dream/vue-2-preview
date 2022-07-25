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
      pdfUrl: ''
    }
  },
  watch: {
    'item.show': {
      handler: function () {
        this.audioList[0].url        = this.item.url
        this.aliplayer.source        = this.item.url
        this.pdfUrl                  = this.item.url
        this.imgUrl                  = this.item.url
        this.aliplayer.options.cover = this.item.cover
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
              <pdf src={this.pdfUrl}></pdf>
            </div>
        break
    }
    return (
        <el-dialog
            title={this.item.title}
            visible={this.item.show}
            {...{on: {'update:visible': val => this.item.show = val}}}
            append-to-body
            width="80%">{temp}</el-dialog>)
  },
  methods: {
  }
}
</script>

<style lang="less" scoped>
::v-deep .el-carousel--horizontal {
  overflow: hidden;
}

::v-deep {
  .audio__play-rate__dropdown {
    width: 2rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}

.placeholder {
  height: 5rem;
  width: 5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
