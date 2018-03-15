<template>
  <section class="dialog-wrap " :class="{active:show}">
    <div class="overlay"  @click="close(0)"></div>
    <div class="dialog">
      <div class="dialog-hd" v-if="title">{{title}}</div>
      <div class="dialog-bd">
        <slot name="content"></slot>
      </div>
      <footer class="dialog-ft">
        <slot name="button">
          <a href="javascript:;" class="ft-btn" :class="cancelButtonClass" v-if="showCancelButton" @click="close(1)">取消</a>
          <a href="javascript:;" class="ft-btn" :class="confirmButtonClass" v-if="showConfirmButton" @click="submit">{{confirmButtonText}}</a>
        </slot>
      </footer>
    </div>
  </section>
</template>
<script type="text/ecmascript-6">
  export default {
    name: 'demo',
    props: {
      title: {
        default: '',
      },
      showCancelButton:{
        default:false
      },
      cancelButtonClass:{
        default:'ft-btn__default'
      },
      cancelButtonText:{
        default:'取消'
      },
      showConfirmButton:{
        default:true
      },
      confirmButtonClass:{
        default:'ft-btn__active'
      },
      confirmButtonText:{
        default:'确定'
      }
    },
    data() {
      return {
        show: false,   // 是否显示模态框
        resolve: '',
        reject: '',
        promise: '',  // 保存promise对象
      };
    },
    computed: {

    },
    mounted() {

    },
    methods: {
      /**
       * 确定,将promise断定为完成态
       */
      submit() {
        this.resolve('submit');
      },
      /**
       * 关闭,将promise断定为reject状态
       * @param type {number} 关闭的方式 0表示关闭按钮关闭,1表示取消按钮关闭
       */
      close(type) {
        this.show = false;
        this.reject(type);
      },
      /**
       * 显示confirm弹出,并创建promise对象
       * @returns {Promise}
       */
      confirm() {
        this.show = true;
        this.promise = new Promise((resolve, reject) => {
          this.resolve = resolve;
          this.reject = reject;
        });
        return this.promise;   //返回promise对象,给父级组件调用
      },
    },
    components: {},
    filters: {}
  }
</script>
<style>
  .dialog-wrap {
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
    -webkit-justify-content: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -webkit-align-items: center;
    -ms-flex-align: center;
    align-items: center;
    z-index: 4000;
    display: none;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -webkit-flex-direction: column;
    -ms-flex-direction: column;
    flex-direction: column;
  }

  .dialog-wrap.active {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
  }

  .overlay {
    display: none;
    background: rgba(0, 0, 0, 0.7);
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: 4000;
  }

  .active .overlay, .overlay.active {
    display: block;
  }

  .dialog {
    position: relative;
    width: 280px;
    background: #fff;
    border-radius: 5px;
    overflow: hidden;
    z-index: 4005;
  }

  .dialog .dialog-hd {
    padding: 10px 10px 0 10px;
    font-size: 16px;
    font-weight: bold;
    line-height: 1.3;
    text-align: center;
  }

  .dialog .dialog-bd {
    padding: 20px 10px;
  }

  .dialog .dialog-bd .bd-tt {

  }

  .dialog .dialog-bd .bd-txt.txt--left {
    text-align: left;
  }

  .dialog .dialog-bd .bd-txt.txt--center {
    text-align: center;
  }

  .dialog .dialog-ft {
    overflow: hidden;
    text-align: center;
    line-height: 44px;
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
  }

  .dialog .dialog-ft .ft-btn {
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    -ms-flex: 1;
    flex: 1;
    width: 1%;
  }

  .dialog .dialog-ft .ft-btn {
    position: relative;
    color: #333;
    font-size: 15px;
    text-decoration: none;
  }
  .dialog .dialog-ft .ft-btn:hover{
    color: #333;
  }

  .dialog .dialog-ft .ft-btn:first-of-type {
    border-bottom-left-radius: 5px;
  }

  .dialog .dialog-ft .ft-btn::before {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    box-sizing: border-box;
    right: 0;
    height: 0;
    -webkit-transform: scaleY(0.5);
    -ms-transform: scaleY(0.5);
    transform: scaleY(0.5);
    border-top: 1px solid #dbdbdb;
  }

  .dialog .dialog-ft .ft-btn:nth-of-type(1)::after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    box-sizing: border-box;
    width: 0;
    bottom: 0;
    -webkit-transform: scaleX(0.5);
    -ms-transform: scaleX(0.5);
    transform: scaleX(0.5);
    border-left: 1px solid #dbdbdb;
    left: auto;
    right: 0;
  }

  .dialog .dialog-ft .ft-btn:last-of-type {
    border-bottom-right-radius: 5px;
  }
  .dialog .dialog-ft .ft-btn__active,.dialog .dialog-ft .ft-btn__active:hover{
    font-size: 15px;
    color: #FFA400;
  }
</style>
