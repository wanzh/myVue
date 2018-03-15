<template>
  <div class="stepper">
    <span class="stepper-minus" @click="minus"></span>
    <span class="stepper-num">{{number}}</span>
    <!--<input type="text" class="stepper-num" v-model="number" @keyup="fixNumber">-->
    <span class="stepper-plus" @click="add"></span>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: {
      max: {
        type: Number,
        default: 5
      },
      min: {
        type: Number,
        default: 1
      }
    },
    data () {
      return {
        number: this.min
      }
    },
    watch: {
      number () {
        this.$emit('onChange', this.number)
      }
    },
    methods: {
      fixNumber () {
        let fix
        if (typeof this.number === 'string') {
          fix = Number(this.number.replace(/\D/g, ''))
        }
        else {
          fix = this.number
        }
        if (fix > this.max || fix < this.min) {
          fix = this.min
        }
        this.number = fix
      },
      minus () {
        if (this.number <= this.min) {
          return
        }
        this.number --
      },
      add () {
        if (this.number >= this.max) {
          return
        }
        this.number ++
      }
    }
  }
</script>
<style scoped>
  .stepper {
    position: relative;
    padding: 0 14px;
    width: 60px;
    background: #fff;
    border-radius: 5px;
  }

  .stepper .stepper-num {
    display: inline-block;
    width: 100%;
    height: 30px;
    line-height: 30px;
    font-size: 15px;
    color: #222222;
    text-align: center;
  }

  .stepper .stepper-plus, .stepper .stepper-minus {
    position: absolute;
    top: 8px;
    height: 14px; /*no*/
    width: 14px; /*no*/
    border-radius: 50%;
    overflow: hidden;
  }

  .stepper .stepper-plus {
    right: 0;
    background: #FF6500;
    color: #fff;
  }

  .stepper .stepper-minus {
    left: 0;
    background: #fff;
    border: 1px solid #dbdbdb; /*no*/
    color: #FF6500;
  }

  .stepper .stepper-minus::before {
    content: "";
    height: 2px;
    width: 8px;
    background: currentColor;
    position: absolute;
    top: 50%;
    left: 50%;
    -webkit-transform: translate3d(-50%, -50%, 0);
    transform: translate3d(-50%, -50%, 0);
  }

  .stepper .stepper-plus::before, .stepper .stepper-plus::after {
    content: "";
    height: 2px;
    width: 8px;
    background: currentColor;
    position: absolute;
    top: 50%;
    left: 50%;
    -webkit-transform: translate3d(-50%, -50%, 0);
    transform: translate3d(-50%, -50%, 0);
  }

  .stepper .stepper-plus::after {
    height: 8px;
    width: 2px;
  }
</style>
