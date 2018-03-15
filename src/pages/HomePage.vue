<template>
    <div>
      <w-range :min="0" :max="10" :unit="'万'" ref="moneyRange"></w-range>
      <w-stepper :max="100" :min="1" ref="stepper" @onChange="onNumberChange"></w-stepper>
      <div><button type="button" @click="showDialog" style="margin-bottom: 20px;">打开dialog</button></div>
      <w-dialog title="什么是定金？" confirmButtonText="我知道了" ref="dialog">
        <div slot="content" class="djDialogContent">
          <p>1、100元定金，为您锁住变动价格，余额支付时<strong>可抵200元</strong></p>
          <p>2、下单后，<strong>余额需要在3个工作日内完成支付</strong>，3日内定金可退。超过3日，订单失效。定金不再支持退款。</p>
        </div>
      </w-dialog>
      <calendar date="2018/06/13 00:00:00"></calendar>
    </div>
</template>
<script type="text/ecmascript-6">
  import WRange from '@/components/Range'
  import WStepper from '@/components/Stepper'
  import WDialog from '@/components/Dialog'
  import  Calendar  from '@/components/Calendar'
    export default {
        name: 'demo',
        data() {
            return {
              number:1
            }
        },
        mounted() {


        },
        methods: {
          onNumberChange(num){
            console.log(num);
            this.number=num;
          },
          showDialog(){
            this.$refs.dialog.confirm().then(() => {
              // 点击确定按钮的回调处理
              //alert(1);
              this.$refs.dialog.show = false;
            }).catch(() => {
              // 点击取消按钮的回调处理
            });
          },
        },
        components: {
          WRange,
          WStepper,
          WDialog,
          Calendar
        },
        filters: {}
    }
</script>
<style scoped>
  .djDialogContent {
    font-size: 15px;
    color: #222222;
    line-height: 21px;
  }

  .djDialogContent strong {
    font-size: 15px;
    color: #FF6500;
    font-weight: 400;
  }
</style>
