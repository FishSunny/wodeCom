<template>
  <a-form-model
      :rules="rules"
      layout="horizontal"
      labelAlign="right"
      :labelCol="{ span: 4 }"
      :wrapperCol="{ span: 19 }"
      :model="formData"
      ref="formDataRef"
      v-if="visible"
  >
    <a-form-model-item label="模板名称" prop="templateTitle">
      <ald-input width="373px" v-model="formData.templateTitle" placeholder="请输入模板名称" showWordLimit allow-clear :maxLength="nameMaxLength" />
    </a-form-model-item>
    <a-form-model-item label="订单来源" prop="sourceType">
      <a-select v-model="formData.sourceType" style="width: 120px">
        <a-select-option v-for="(item, index) in sourceTypeList" :key="index" :value="item.dicKey">{{ item.dicVal }}</a-select-option>
      </a-select>
    </a-form-model-item>
    <a-form-model-item label="审核结果" prop="contractState">
      <a-radio-group v-model="auditForm.contractState" prop="contractState" @change="changeContractState">
        <a-radio value="2">通过</a-radio>
        <a-radio value="3">未通过</a-radio>
      </a-radio-group>
    </a-form-model-item>
  </a-form-model>
</template>
<script>
export default {
  name: 'form',
  data() {
    const validateName = (rule, value, callback) =>{
      if(!value){
        callback(new Error('请输入服务商名称'));
      }
      if (!validateMerchant(value)) {
        callback(new Error('请输入中英文格式'));
      } else {
        callback();
      }
    }
    return {
      rules: {
        templateTitle: [{ required: true, message: '请输入模板名称', trigger: 'blur' }],
        templateType: [{ required: true, message: '请选择回复类型', trigger: 'blur' }],
        templateContent: [{ required: true, message: '请输入回复内容', trigger: 'blur' }],
        serviceName: [{ required: true, validator: validateName, trigger: 'blur' }],
      },
      formData: {}
    }
  }
}
</script>


<style scoped lang="less">

</style>
