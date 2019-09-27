<template>
  <div ref="wrapper">笔记本sdsd
    <slot></slot>
  </div>
</template>
 台式机
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'

  export default {
    props: {
      scrollx:{
        type:Boolean,
        default:true
      },
      probeType: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: true
      },
      listenScroll: {
        type: Boolean,
        default: false
      },
      data: {
        type: Array,
        default: null
      },
      pullup: {
        type: Boolean,
        default: false
      },
      beforeScroll: {
        type: Boolean,
        default: false
      },
      refreshDelay: {
        type: Number,
        default: 20
      },
      scrolltoy:{
        type:Number,
        default:0
      }
    },
    mounted() {
      setTimeout(() => {
        this._initScroll()
      }, this.refreshDelay) //20
    },
    methods: {
      _initScroll() {
        if (!this.$refs.wrapper) {
          return
        }
        
        this.scroll = new BScroll(this.$refs.wrapper, {
          scrollX: this.scrollx,
          scrollY:true,
          // eventPassthrough: 'vertical',
          probeType: this.probeType,
          click: this.click,
          useTransition: false,
          preventDefaultException:{tagName: /^(INPUT|TEXTAREA|BUTTON|SELECT|IMG|A)$/}
        })


        // console.log(this.scroll)

        if (this.listenScroll) {
          let me = this
          this.scroll.on('scroll', (pos) => {
            me.$emit('scroll', pos)
            me.$emit('tipsPosDef', pos)
          })
        }

        if (this.pullup) {
          this.scroll.on('scrollEnd', () => {
            if (this.scroll.y <= (this.scroll.maxScrollY + 200)) {
              this.$emit('scrollToEnd')
            }
          })
        }

        if (this.beforeScroll) {
          this.scroll.on('beforeScrollStart', () => {
            this.$emit('beforeScroll')
          })
        }
      },
      disable() {
        this.scroll && this.scroll.disable()
      },
      enable() {
        this.scroll && this.scroll.enable()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      },
      scrollTo() {
        console.log(arguments)
        this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments)
      },
      scrollToElement() {
        this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments)
      }
    },
    watch: {
      data() {
        setTimeout(() => {
          this.refresh()
        }, this.refreshDelay)
      },
      scrolltoy(){
        setTimeout(() => {
         this.scrollTo(0,-this.scrolltoy,0);
        }, this.refreshDelay)
      }
    }
  }
</script>

<style>

</style>
