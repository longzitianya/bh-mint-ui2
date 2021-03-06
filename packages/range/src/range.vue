<template>
  <div class="mt-range" :class="{ 'mt-range--disabled': disabled }">
    <slot name="start"></slot>
    <div class="mt-range-content" ref="content">
      <div class="mt-range-runway mt-bColor-grey-lv4" :style="{ 'border-top-width': barHeight + 'px' }"></div>
      <div class="mt-range-progress mt-bg-primary" :style="{ width: progress + '%', height: barHeight + 'px' }"></div>
      <div class="mt-range-thumb mt-bg-lv3 mt-boxShadow-range-thumb" ref="thumb" :style="{ left: progress + '%' }"></div>
    </div>
    <slot name="end"></slot>
  </div>
</template>

<style>
  .mint-cell-value .mt-range>div:only-child{
    margin-right: 20px;
  }
  @component-namespace mt {
    @component range {
      position: relative;
      display: flex;
      height: 30px;
      line-height: 30px;
      padding-right: 15px;

      & > * {
        display: flex;
        display: -webkit-box;
      }

      & *[slot=start] {
        margin-right: 5px;
      }

      & *[slot=end] {
        margin-left: 5px;
      }
      
      &>div:first-child{
        margin-right: 9px;
      }
      &>div:only-child{
        margin-right: 59px;
      }

      @descendent content {
        position: relative;
        flex: 1;
        margin-right: 39px;
      }

      @descendent runway {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        left: 0;
        right: -28px;
        border-top-style: solid;
        border-radius: 8px;
      }

      @descendent thumb {
        position: absolute;
        left: 0;
        top: 0;
        width: 28px;
        height: 28px;
        border-radius: 100%;
        cursor: move;
      }

      @descendent progress {
        position: absolute;
        display: block;
        top: 50%;
        transform: translateY(-50%);
        width: 0;
        border-radius: 8px;
      }

      @modifier disabled {
        opacity: 0.5;
      }
    }
  }
</style>

<script type="text/babel">
  import draggable from './draggable';

  /**
   * @name Range
   * @tagName mt-range
   * @desc 滑块
   * @wrapClassName mint-range
   * @noteType component
   * @html
   * <mt-range></mt-range>
   */
  export default {
    name: 'mt-range',

    props: {
      /**
       * @noteType prop
       * @field min
       * @desc 最小值
       * @type input
       * @valueType number
       * @value 0
       */
      min: {
        type: Number,
        default: 0
      },
      /**
       * @noteType prop
       * @field max
       * @desc 最大值
       * @type input
       * @valueType number
       * @value 100
       */
      max: {
        type: Number,
        default: 100
      },
      /**
       * @noteType prop
       * @field step
       * @desc 步长
       * @type input
       * @valueType number
       * @value 1
       */
      step: {
        type: Number,
        default: 1
      },
      /**
       * @noteType prop
       * @field disabled
       * @desc 是否禁用
       * @valueType boolean
       * @type radio
       * @value false
       * @option
       [{
           "text": "否",
           "value": false
       },{
           "text": "是",
           "value": true
       }]
       */
      disabled: {
        type: Boolean,
        default: false
      },
      /**
       * @noteType prop
       * @field value
       * @desc 默认值
       * @type textarea
        */
      value: {
        type: Number
      },
      /**
       * @noteType prop
       * @field barHeight
       * @desc 滑槽的线宽
       * @type input
       * @valueType number
       * @placeholder 1
       */
      barHeight: {
        type: Number,
        default: 2
      }
    },

    computed: {
      progress() {
        const value = this.value;
        if (typeof value === 'undefined' || value === null) return 0;
        return Math.floor((value - this.min) / (this.max - this.min) * 100);
      }
    },

    mounted() {
      const thumb = this.$refs.thumb;
      const content = this.$refs.content;

      const getThumbPosition = () => {
        const contentBox = content.getBoundingClientRect();
        const thumbBox = thumb.getBoundingClientRect();
        return {
          left: thumbBox.left - contentBox.left,
          top: thumbBox.top - contentBox.top,
          thumbBoxLeft: thumbBox.left
        };
      };

      let dragState = {};
      draggable(thumb, {
        start: (event) => {
          if (this.disabled) return;
          const position = getThumbPosition();
          const thumbClickDetalX = event.clientX - position.thumbBoxLeft;
          dragState = {
            thumbStartLeft: position.left,
            thumbStartTop: position.top,
            thumbClickDetalX: thumbClickDetalX
          };
        },
        drag: (event) => {
          if (this.disabled) return;
          const contentBox = content.getBoundingClientRect();
          const deltaX = event.pageX - contentBox.left - dragState.thumbStartLeft - dragState.thumbClickDetalX;
          const stepCount = Math.ceil((this.max - this.min) / this.step);
          const newPosition = (dragState.thumbStartLeft + deltaX) - (dragState.thumbStartLeft + deltaX) % (contentBox.width / stepCount);

          let newProgress = newPosition / contentBox.width;

          if (newProgress < 0) {
            newProgress = 0;
          } else if (newProgress > 1) {
            newProgress = 1;
          }

          this.$emit('input', Math.round(this.min + newProgress * (this.max - this.min)));
        },
        end: () => {
          if (this.disabled) return;
          this.$emit('change', this.value);
          dragState = {};
        }
      });
    }
  };
/**
 * @noteType external
 * @content
 {
   "mock": {
     "value": 88,
     "min": 0,
     "max": 100,
     "step": 1
   },
   "bindField": "v-model"
 }
 */
</script>
