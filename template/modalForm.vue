<template>
  <a-modal
      :visible="visible"
      :maskClosable="false"
      centered
      :width="870"
      @cancel="cancelModal"
      @ok="submitModal"
      title="">
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
        <!-- 输入框 -->
        <ald-input width="373px" v-model="formData.templateTitle" placeholder="请输入模板名称" showWordLimit allow-clear :maxLength="nameMaxLength" />
      </a-form-model-item>
      <a-form-model-item label="订单来源" prop="sourceType">
        <!-- 下拉框 -->
        <ald-select v-model="formData.sourceType" style="width: 120px">
          <a-select-option v-for="(item, index) in sourceTypeList" :key="index" :value="item.dicKey">{{ item.dicVal }}</a-select-option>
        </ald-select>
      </a-form-model-item>
      <!-- 下拉框带搜索-->
      <a-form-model-item class="ald-flex ald-col-top" label="负责人" prop="userList">
        <a-select mode="multiple" show-search style="width:300px" v-model="formData.userList" placeholder="请选择负责人" :filter-option="filterOption">
          <a-select-option v-for="(item, index) in userList" :key="index" :value="item.userId">
            {{ item.nickName }}
          </a-select-option>
        </a-select>
      </a-form-model-item>
      <a-form-model-item label="审核结果" prop="contractState">
        <!-- 单选框 -->
        <a-radio-group v-model="formData.contractState" prop="contractState">
          <a-radio value="2">通过</a-radio>
          <a-radio value="3">未通过</a-radio>
        </a-radio-group>
      </a-form-model-item>
      <a-form-model-item class="ald-flex ald-col-top" label="备注" prop="remark">
        <!-- 长文本 -->
        <ald-textarea width="300px" v-model="formData.remark" :maxLength="100" :showWordLimit="true"  :placeholder="`请输入备注`" :auto-size="{ minRows: 5, maxRows: 6 }" allowClear />
      </a-form-model-item>
    </a-form-model>
    <template slot="footer">
      <a-button key="back" @click="cancelModal">取消</a-button>
      <a-button key="submit" :loading="loading" type="primary" @click="submitModal">确定</a-button>
    </template>
  </a-modal>
</template>
<script>
export default {
  name: '',
  data() {
    return {
      visible: true,
      loading: false,
      nameMaxLength: 30,
      rules: {
        templateTitle: [{ required: true, message: '请输入模板名称', trigger: 'blur' }],
        templateType: [{ required: true, message: '请选择回复类型', trigger: 'blur' }],
        templateContent: [{ required: true, message: '请输入回复内容', trigger: 'blur' }],
        serviceName: [{ required: true, message: '请输入服务名称', trigger: 'blur' }],
      },
      userList: [],
      formData: {
        remark: '',
        contractState: ''

      },
      sourceTypeList: []
    }
  },
  methods: {
    filterOption(input, option) {
      return (
          option.componentOptions.children[0].text.toLowerCase().indexOf(input.toLowerCase()) >= 0
      );
    },
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
