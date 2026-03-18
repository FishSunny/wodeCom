<template>
  <common-search>
    <a-form-model ref="searchForm" :model="searchForm" class="search-content" layout="inline">
      <a-form-model-item label="订单时间" prop="createTimeArr">
        <ald-range-picker @change="changeTime($event, 'createTimeArr')" v-model="searchForm.createTimeArr" :valueFormat="dateFormat" allow-clear />
      </a-form-model-item>
      <a-form-model-item label="酒店名称" prop="hotelId">
        <hotelSearch v-model="searchForm.hotelId"/>
      </a-form-model-item>
      <a-form-model-item label="预订人" prop="linkName">
        <ald-input v-model="searchForm.linkName" placeholder="请输入预订人姓名" allow-clear />
      </a-form-model-item>
      <a-form-model-item label="预订人手机号" prop="userPhone">
        <ald-input
            allow-clear
            v-model="searchForm.userPhone"
            :maxLength="11"
            @input="searchForm.userPhone=searchForm.userPhone.replace(/[^\d]/g,'')"
            style="width: 180px"
            placeholder="请输入预订人手机号"
        />
      </a-form-model-item>
      <a-form-model-item label="订单来源" prop="sourceType">
        <a-select v-model="searchForm.sourceType" style="width: 120px">
          <a-select-option :key="-1" value="">全部</a-select-option>
          <a-select-option v-for="(item, index) in sourceTypeList" :key="index" :value="item.dicKey">{{ item.dicVal }}</a-select-option>
        </a-select>
      </a-form-model-item>
      <a-form-model-item label="城市" prop="areaCode">
        <select-city ref="selectCity" :desc = "'请选择城市'" :level = "true" @changeAreaCode="changeSelectAreaCode" :default-area-code="searchForm.areaCode"></select-city>
      </a-form-model-item>
      <!-- 下拉搜索 -->
      <a-form-model-item label="类型名称" prop="projectTypeId">
        <a-select
            show-search
            :value="searchForm.projectTypeId"
            placeholder="请输入类型名称"
            style="width: 216px"
            :default-active-first-option="false"
            :show-arrow="false"
            :filter-option="false"
            :not-found-content="null"
            @search="searchType"
            @change="handleChange"
        >
          <a-select-option v-for="(item, index) in typeList" :key="index" :value="item.projectTypeId">
            {{ item.typeName }}
          </a-select-option>
        </a-select>
      </a-form-model-item>
      <a-form-model-item>
        <a-button :loading="loading" @click="searchList()" type="primary">搜索</a-button>
        <a-button class="ald-ml8" :loading="loading" @click="resetSearch()">重置</a-button>
        <a-button class="ald-ml8" :loading="exportLoading" @click="onExportExcel()">导出</a-button>
      </a-form-model-item>
    </a-form-model>
  </common-search>
</template>

<script>
export default {
  name: 'search',
  data() {
    return {
      dateFormat: 'YYYY-MM-DD',
      exportLoading: false,
      loading: false,
      searchForm: {},
      searchFormVal: {},
      pageInfo: {
        pageNum: 1,
        pageSize: 10,
        pageFlag: 1
      },
      dataList: [],
      total: 0
    }
  },
  methods: {
    searchList() {
      this.pageInfo.pageNum = 1;
      this.searchFormVal = Object.assign(this.searchFormVal, this.searchForm)
      this.getXXXList();
    },
    // 点击清空搜索条件按钮
    resetSearch() {
      this.$refs.searchForm.resetFields();
      this.searchList();
    },
    async getXXXList() {
      let postData = {
        ...this.searchFormVal,
        ...this.pageInfo
      }
      this.loading = true;
      let res = await XXX(postData);
      if (res.code !== '0') {
        this.$message.error(res.errorMsg);
        this.loading = false;
        return;
      }
      this.loading = false;
      this.dataList = res.data.list;
      this.total = res.data.count;
    },
    onExportExcel() {
      //导出
      if (this.dataList.length == 0) {
        this.$message.error("数据为空无法导出");
        return;
      }
      this.exportLoading = true;
      let interfaceUrl = '';
      try {
        exportExcel(this.searchFormVal, interfaceUrl, "海科支付订单").then(() => {
          this.exportLoading = false;
        });
      } catch () {
        this.exportLoading = false;
      }
    }
  },
  created() {
    this.searchList();
  }
}
</script>



<style scoped lang="less">

</style>
