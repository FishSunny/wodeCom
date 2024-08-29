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
    </template>å
  </a-modal>
  <Component v-if="" @cancelModal="" @submitModal=""></Component>
</template>
<script>
export default {
  name: '',
  data() {
    return {
      visible: true,
    }
  },
  methods: {
    cancelModal() {
      this.$refs.formDataRef.resetFields();
      this.$emit('cancelModal');
    },
    submitModal() {
      this.$emit('submitModal');
    }
  }
}
</script>


<style scoped lang="less">

</style>
