<!--
  * @Author       : 樊小渝
  * @Date         : 2024-02-29 16:24:30
  * @LastEditors  : 樊小渝
  * @LastEditTime : 2024-02-29 16:24:30
  * @FilePath     : modal.vue
  * @Description  : '弹窗组件'
-->
<template>
  <a-modal
      :visible="visible"
      :maskClosable="false"
      centered
      :width="870"
      @cancel="cancelModal"
      @ok="submitModal"
      title="">
    <template slot="footer">
      <a-button key="back" @click="cancelModal">取消</a-button>
      <a-button key="submit" :loading="loading" type="primary" @click="submitModal">确定</a-button>
    </template>
  </a-modal>
  <Component v-if="" @cancelModal="" @submitModal=""></Component>
</template>
<script>
export default {
  name: '',
  data() {
    return {
      visible: true,
      loading: false,
    }
  },
  methods: {
    cancelModal() {
      this.$refs.formDataRef.resetFields();
      this.$emit('cancelModal');
    },
    submitModal() {
      this.$refs.formDataRef.validate(async (valid) => {
        if (valid) {
          this.loading = true;
          let res = null;
          if (this.isEdit) {
            res = await XXXApi.editPaySetting(this.formData);
          } else {
            res = await XXXApi.addPaySetting(this.formData)
          }
          if (res.code !== '0') {
            this.loading = false;
            this.$message.error(res.errorMsg);
            return;
          }
          this.loading = false;
          this.$message.success(`${this.isEdit ? '编辑' : '添加'}收款渠道成功`);
          this.$emit('submitModal');
        }
      })
    }
  }
}
</script>


<style scoped lang="less">

</style>
