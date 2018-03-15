<template>
  <div class="range" ref="rangeBox">
    <div class="range-content">
      <!--<div>
        <div class="range-runway" style="border-top-width: 1px;"></div>
        <div class="range-progress" style="width: 20%; height: 1px;"></div>
        <div class="range-thumb" style="left: 20%;"></div>
      </div>-->
      <div class="range-runway" style="height: 6px;"></div>
      <div class="range-progress" style="height: 6px;left: 0;width: 100%" ref="progress"></div>
      <div class="range-start" v-show="startValue!=min">{{min}}<span v-if="min!=0">{{unit}}</span></div>
      <div class="range-end" v-show="endValue!=max">{{max}}{{unit}}</div>

      <div class="range-thumb range-thumb__left" :style="{left: left+'px'}" ref="left">
        <i class="range-thumb-dot"></i>
        <div class="range-thumb-txt">{{startValue}}<span v-if="startValue!=0">{{unit}}</span></div>
      </div>
      <div class="range-thumb range-thumb__right" :style="{left: right+'px'}" ref="right">
        <i class="range-thumb-dot"></i>
        <div class="range-thumb-txt">{{endValue}}{{unit}}</div>
      </div>

    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    name: 'demo',
    props: {
      max: {
        type: Number,
        required: true
      },
      min: {
        type: Number,
        required: true
      },
      unit: {
        default: ''
      }
    },
    data() {
      return {
        left: 0,
        right: 0,
        width: 0,
        thumbWidth: 26
      }
    },
    mounted() {
      this.drag('left');
      this.drag('right');
      this.width = this.$refs['rangeBox'].clientWidth;
      this.thumbWidth = this.$refs['left'].offsetWidth;
      this.left = -(this.thumbWidth / 2);
      this.right = this.width - this.thumbWidth / 2;
    },
    watch: {
      left(){

      }
    },
    computed: {
      startValue(){
        let v = ((this.left + this.thumbWidth / 2) / this.width * ((this.max - this.min)) + this.min).toFixed(2);
        if (v == this.min.toFixed(2)) {
          v = this.min;
        }
        if (v == this.max.toFixed(2)) {
          v = this.max;
        }
        return v;
      },
      endValue(){
        let v = ((this.right + this.thumbWidth / 2) / this.width * ((this.max - this.min)) + this.min).toFixed(2);
        if (v == this.min.toFixed(2)) {
          v = this.min;
        }
        if (v == this.max.toFixed(2)) {
          v = this.max;
        }
        return v;
      }
    },
    methods: {
      drag(type){
        var oW, oH;
        let that = this;
        let block = '';
        let rangeProgress = this.$refs['progress'];
        if (type == 'left') {
          block = this.$refs['left'];
          this.$refs['right'].style.zIndex = 1;
        } else {
          block = this.$refs['right'];
          this.$refs['right'].style.zIndex = 1;
        }
        let rangeBox = this.$refs['rangeBox'];
        block.addEventListener("touchstart", function (e) {
          var touches = e.touches[0];
          oW = touches.clientX - block.offsetLeft;
          oH = touches.clientY - block.offsetTop;
          //阻止页面的滑动默认事件
          document.addEventListener("touchmove", defaultEvent, false);
        }, false);

        block.addEventListener("touchmove", function (e) {
          var touches = e.touches[0];
          var oLeft = touches.clientX - oW;
          var oTop = touches.clientY - oH;
          //console.log('oLeft',oLeft);
          //console.log('that.left',that.left);
          //console.log('that.right',that.right);
          if (oLeft < -(that.thumbWidth / 2)) {
            oLeft = -(that.thumbWidth / 2);
          } else if (oLeft > rangeBox.clientWidth - (that.thumbWidth / 2)) {
            oLeft = (rangeBox.clientWidth - (that.thumbWidth / 2));
          }

          if (type == 'left') {
            //that.left = oLeft;
            if (oLeft > that.right) {
              that.left = that.right;
            } else {
              that.left = oLeft;
            }
            block.style.left = that.left + "px";
            rangeProgress.style.left = that.left + "px";
            rangeProgress.style.width = (that.right - that.left) + "px";
          } else {
            //that.right = oLeft;
            if (oLeft < that.left) {
              that.right = that.left;
            } else {
              that.right = oLeft;
            }
            block.style.left = that.right + "px";
            rangeProgress.style.width = (that.right - that.left) + "px";
          }
          document.addEventListener("touchmove", defaultEvent, false);
        }, false);

        block.addEventListener("touchend", function (e) {
          document.removeEventListener("touchmove", defaultEvent, false);
        }, false);
        function defaultEvent(e) {
          e.preventDefault();
        }
      }
    },
    components: {},
    filters: {}
  }
</script>
<style>
  .range {
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    height: 78px;
    margin: 0 13px 0 13px;
    line-height: 30px
  }

  .range > * {
    display: -ms-flexbox;
    display: flex;
    display: -webkit-box
  }

  .range *[slot=start] {
    margin-right: 5px
  }

  .range *[slot=end] {
    margin-left: 5px
  }

  .range-content {
    position: absolute;
    top: 24px;
    left: 0;
    right: 0;
  }

  .range-runway {
    position: absolute;
    top: 13px;
    left: 0;
    right: 0;
    background: #CECECE;
    border-radius: 3px;
  }

  .range-thumb {
    position: absolute;
    top: 0;
    cursor: move;
    /*height: 50px;*/
  }

  .range-start {
    position: absolute;
    left: 0;
    top: 36px;
    height: 18px;
    line-height: 18px;
  }

  .range-end {
    position: absolute;
    right: 0;
    top: -20px;
    height: 18px;
    line-height: 18px;
  }

  .range-thumb-dot {
    display: block;
    width: 26px;
    height: 26px;
    background-color: #fff;
    border-radius: 100%;
    box-shadow: 0 1px 3px rgba(0, 0, 0, .4);
    /* background: url("../../static/img/icon-range.png") no-repeat 100% 50%;
     background-size: 18px auto;*/
  }

  .range-thumb-txt {
    position: absolute;
    bottom: 0;
    left: 50%;
    /* width: 80px;*/
    height: 18px;
    padding: 0 10px;
    line-height: 18px;
    -webkit-transform: translate(-50%, 0);
    transform: translate(-50%, 0);
    background: #fff;
    /*background: #F4F4F4;*/
    text-align: center;
    white-space: nowrap;
    font-size: 14px;
  }

  .range-thumb__left .range-thumb-txt {
    top: 36px;
  }

  .range-thumb__right .range-thumb-txt {
    top: -20px;
  }

  .range-progress {
    position: absolute;
    display: block;
    background-color: #FFB900;
    top: 13px;
    border-radius: 3px;
    width: 0
  }

  .range--disabled {
    opacity: 0.5
  }

</style>
