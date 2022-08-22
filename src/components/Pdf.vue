<template>
  <div>
    <div class="pdf_down">
      <div class="pdf_set_left" @click="scaleD()">放大</div>
      <div class="pdf_set_middle" @click="scaleX()">缩小</div>
    </div>
    <div v-if="src" :style="{width:pdfDivWidth,margin:'0 auto'}">
      <canvas v-for="page in pages" :id="'the-canvas'+page" :key="page"></canvas>
    </div>
    <div v-else class="empty">空</div>
  </div>
</template>

<script>
const PDFJS = require('pdfjs-dist')
export default {
  name: 'Pdf',
  props: {
    src: {
      type: String,
      default: () => ''
    },
  },
  data() {
    return {
      max: 10,
      min: 1,
      scale: 2.0,//pdf放大系数
      pages: [],
      pdfDivWidth: '',
    }
  },
  mounted() {
    this.src && this.loadFile(this.src)
  },
  methods: {
    scaleD() {  //放大
      if (this.scale >= this.max) {
        return
      }
      this.scale = this.scale + 0.1
      this.loadFile(this.src)
    },
    scaleX() {  //缩小
      if (this.scale <= this.min) {
        return
      }
      this.scale = this.scale - 0.1
      this.loadFile(this.src)
    },
    loadFile(url) {  //初始化pdf
      let loadingTask = PDFJS.getDocument(url)
      loadingTask.promise.then((pdf) => {
        this.pdfDoc = pdf
        this.pages  = this.pdfDoc.numPages
        this.$nextTick(() => {
          this._renderPage(1)
        })
      })
    },
    _renderPage(num) {  //渲染pdf页
      const that = this
      this.pdfDoc.getPage(num).then((page) => {
        let canvas   = document.getElementById('the-canvas' + num)
        let ctx      = canvas.getContext('2d')
        let dpr      = window.devicePixelRatio || 1
        let bsr      = ctx.webkitBackingStorePixelRatio ||
            ctx.mozBackingStorePixelRatio ||
            ctx.msBackingStorePixelRatio ||
            ctx.oBackingStorePixelRatio ||
            ctx.backingStorePixelRatio || 1
        let ratio    = dpr / bsr
        let viewport = page.getViewport({scale: this.scale})

        canvas.width        = viewport.width * ratio
        canvas.height       = viewport.height * ratio
        canvas.style.width  = viewport.width + 'px'
        that.pdfDivWidth    = viewport.width + 'px'
        canvas.style.height = viewport.height + 'px'
        ctx.setTransform(ratio, 0, 0, ratio, 0, 0)
        let renderContext = {
          canvasContext: ctx,
          viewport: viewport
        }
        page.render(renderContext)
        if (this.pages > num) {
          this._renderPage(num + 1)
        }
      })
    },
  }
}
</script>

<style scoped>
.pdf_down {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}

.pdf_set_left {
  margin-right: 1rem
}

.pdf_set_left, .pdf_set_middle {
  color: skyblue;
  cursor: pointer;
}

.empty {
  font-size: 18rem;
  color: #5d5c5c;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
