<template>
  <div class="marquee" :style="{'font-size': fontSize, 'height': height, 'line-height': height}">
    <i class="fa fa-fw fa-volume-up"></i>
    <div class="wrap" :style="{'font-size': fontSize, 'height': height, 'line-height': height}">
      <div class="content" :style="{'font-size': fontSize,'height': height, 'line-height': height,'width': this.width + 'px', 'animation-duration': (this.width / 36) + 's'}">
        {{text}}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'JMarquee',
  props: {
    lists: {
      type: Array,
      default () {
        return []
      }
    },
    fontSize: {
      type: String,
      default: ''
    },
    height: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      text: '',
      width: ''
    }
  },
  watch: {
    lists: {
      handler () {
        const size = this.getTextSize(this.lists.reduce((prev, v) => prev + v, ''), '0.24rem')
        this.text = size.text
        this.width = size.width
      },
      deep: true
    }
  },
  methods: {
    getTextSize (text, fontSize) {
      text = text.replace(/<[^<>]+>/g, '')
      text = text.replace('\n', '')
      const span = document.createElement('span')
      const result = {}
      result.width = span.offsetWidth
      result.height = span.offsetHeight
      span.style.visibility = 'hidden'
      span.style.fontSize = fontSize
      span.style.display = 'inline-block'
      span.style.whiteSpace = 'nowrap'
      document.body.appendChild(span)
      span.innerHTML = text
      result.text = text
      result.width = parseFloat(window.getComputedStyle(span).width) - result.width
      result.height = parseFloat(window.getComputedStyle(span).height) - result.height
      document.body.removeChild(span)
      return result
    }
  }
}
</script>
<style lang="less" scoped>
  .marquee {
    width: 100%;
    display: flex;
    display: -webkit-box;
    display: -webkit-flex;
    align-items: center;
    padding: 0.1rem;
    box-sizing: border-box;
    /*border-bottom: 0.01rem solid #d8d8d8;*/
    background-color: @theme-bg-color;

    .fa {
      /*margin-right: 0.1rem;*/
      color: red;
    }
  }

  .wrap {
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    width: 100%;
    overflow: hidden;
    position: relative;
  }

  .content {
    position: relative;
    animation-name: marquee-animation;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
  }

  @keyframes marquee-animation {
    from {
      transform: translateX(10%);
    }
    to {
      transform: translateX(-100%);
    }
  }

</style>
