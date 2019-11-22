/*
* author: mamingyang@baofeng.com
* date: 2018/10/25
* usage: 获取验证码插件
* 接收参数：
* type: 验证码类型；
* phone: 手机号；
* disableBtn: Boolean格式，是否采用文字形式的“获取验证码”，若为否或不传，则为按钮形式；
* np: necessary params，Array格式，必要参数，可不传。若传，则校验必要参数是否存在，若不存在则不执行获取验证码操作。
*/
<template>
  <el-button round :type="disableBtn?'text':'primary'" :class="[{limitWidth:disableBtn},{noMargin:this.text.length>5}]"
             :disabled="sending" @click="countDown">
    {{text}}
  </el-button>
</template>

<script>
export default {
  name: 'VerifyCode',
  props: ['type', 'phone', 'disableBtn', 'np'],
  data() {
    return {
      text: '获取验证码',
      sending: false,
    };
  },
  methods: {
    async countDown() {
      // 校验是否重复点击
      if (this.sending) {
        return;
      }
      // 结束
      // 校验手机号位数
      if (this.phone && this.phone.length !== 11) {
        return;
      }
      // 结束
      // 校验必要参数是否存在
      if (!this.type) {
        return;
      }
      if (this.np && this.np.length) {
        for (let i = 0; i < this.np.length; i++) {
          if (!this[this.np[i]]) {
            return;
          }
        }
      }
      // 结束
      this.sending = true;
      let time = 120;
      this.text = `重新获取(${time})`;
      const j = setInterval(() => {
        if (time > 0) {
          time--;
          this.text = `重新获取(${time})`;
        } else {
          this.text = '获取验证码';
          this.sending = false;
          clearInterval(j);
        }
      }, 1000);
      this.$store.dispatch('global/sendVc', {
        phone: this.phone,
        type: this.type,
      }).then(() => {
        this.$message({
          type: 'success',
          message: '验证码已发放，请注意查收',
        });
      }).catch((message) => {
        this.$message({
          type: 'error',
          message,
        });
        this.text = '获取验证码';
        this.sending = false;
        clearInterval(j);
      });
    },
  },
};
</script>

<style lang="scss" scoped="scoped">
  button {
    transition: none !important;
  }

  .limitWidth {
    padding: 0;
    margin: 0 8px;
  }

  .noMargin {
    margin: 0;
  }
</style>
