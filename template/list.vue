<template>
  <div>
    <common-search>
      <a-form-model ref="searchForm" :model="searchForm" class="search-content" layout="inline">
        <a-form-model-item label="订单时间" prop="createTimeArr">
          <ald-range-picker v-model="searchForm.createTimeArr" :valueFormat="dateFormat" allow-clear />
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
          <ald-select v-model="searchForm.sourceType" style="width: 120px">
            <a-select-option :key="-1" value="">全部</a-select-option>
            <a-select-option v-for="(item, index) in sourceTypeList" :key="index" :value="item.dicKey">{{ item.dicVal }}</a-select-option>
          </ald-select>
        </a-form-model-item>
        <!-- 下拉搜索 -->
        <a-form-model-item label="类型名称" prop="projectTypeId">
          <ald-select
              v-model="searchForm.region"
              placeholder="请选择"
              allow-clear
          >
            <a-select-option
                v-for="(item, index) in regionList"
                :key="index"
                :value="item.dicKey"
            >
              {{ item.dicVal }}
            </a-select-option>
          </ald-select>
        </a-form-model-item>
        <a-form-model-item>
          <a-button :loading="loading" @click="searchList()" type="primary">搜索</a-button>
          <a-button class="ald-ml8" :loading="loading" @click="resetSearch()">重置</a-button>
          <a-button class="ald-ml8" :loading="exportLoading" @click="onExportExcel()">导出</a-button>
        </a-form-model-item>
      </a-form-model>
    </common-search>
    <div class="ald-common-container ald-mt12">
      <ald-table
          title="XXX"
          :actions="'false'"
          :tableIndex="true"
          rowKey=""
          :loading="loading"
          :columns="columns"
          :data-source="dataList"
          :scroll="{x: '100%'}"
          :pagination="{
              current: pageInfo.pageNum,
              pageSize: pageInfo.pageSize,
              total: total,
              showSizeChanger: true,
              showLessItems: true,
              showQuickJumper: true,
              showTotal: (total, range) => `总计 ${total} 条`,
              onChange: (page, pageSize) => {
                ;(pageInfo.pageNum = page), ( pageInfo.pageSize = pageSize), this.getXXXList()
              },
              onShowSizeChange: (current, size) => {
                ;(pageInfo.pageNum = current), (pageInfo.pageSize = size), this.getXXXList()
              },
            }"
      >
        <!-- 表格上方按钮 -->
        <template slot="actions">
          <a-button @click="toAdd()" type="primary">XXX</a-button>
        </template>
        <template slot="fullName" slot-scope="{text}">
          <span style="cursor:pointer;color:#1890ff">{{text.fullName}}</span>
        </template>
        <template slot="createTime" slot-scope="{text, record}">
          <div>{{ record.createUserName }}</div>
          <div>{{ record.createTime ? momentUtil.standardDateTime(record.createTime) : '-' }}</div>
        </template>
        <template slot="updateTime" slot-scope="{text, record}">
          <div>{{ record.updateUserName }}</div>
          <div>{{ record.updateTime ? momentUtil.standardDateTime(record.updateTime) : '-'}}</div>
        </template>
        <!-- 表格操作列按钮 -->
        <template slot="action" slot-scope="{text, record}">
          <a-button size="small" type="link" @click="toEdit(record)">编辑</a-button>
          <a-button size="small" type="link" @click="toOperate(record, record.status === 1)">{{ record.status === 1 ? '停用' : '启用' }}</a-button>
          <a-button size="small" type="link" @click="toDetail(record)">详情</a-button>
          <a-button size="small" type="link" class="danger-btn" @click="toDelete(record)">删除</a-button>
        </template>
      </ald-table>
    </div>
  </div>
</template>

<script>
import * as momentUtil from '@/utils/feature/moment';
import { exportExcel } from '@/utils/feature/export';
export default {
  name: 'XXX',
  data() {
    return {
      momentUtil,
      dateFormat: 'YYYY-MM-DD',
      exportLoading: false,
      loading: false,
      regionList: [],
      searchForm: {
        createTimeArr: [],
        hotelId: '',
        linkName: '',
        userPhone: '',
        sourceType: '',
        region: '',

      },
      searchFormVal: {},
      pageInfo: {
        pageNum: 1,
        pageSize: 10,
        pageFlag: 1
      },
      dataList: [],
      total: 0,
      columns: [
        {
          title: '酒店名称',
          key: 'hotelName',
          scopedSlots: {customRender: 'hotelName'},
          width: 200,
        },
        {
          title: "酒店类型",
          dataIndex: "hotelType",
          width: 120
        },
        {
          title: '入住类型',
          dataIndex: 'checkInType',
          customRender: text => this.$dictUtil.getValueByKey(this.checkInTypeList, text), // 字典转换
          width: 150
        },
        {
          title: '创建人',
          width: 160,
          key: 'createTime',
          scopedSlots: {customRender: 'createTime'},
        },
        {
          title: '操作时间',
          width: 160,
          key: 'updateTime',
          scopedSlots: {customRender: 'updateTime'},
        },
        {
          title: '操作',
          width: 180,
          fixed: 'right',
          key: 'action',
          scopedSlots: {customRender: 'action'},
        }
      ]
    }
  },
  methods: {
    toAdd() {},
    toEdit(item) {},
    toDetail(item) {},
    toOperate(item, ifDisable) {
      this.$confirm({
        title: '提示',
        content: ifDisable ? '确定要下架该XX？' : '确定要上架该XX？',
        okText: '确定',
        cancelText: '取消',
        onOk: async() => {
          await this.operateDisabled(item,ifDisable);
        }
      });
    },
    // 调接口禁用启用
    async operateDisabled(item,ifDisable) {
      let postData = {
        activityId:item.activityId,
        activityState: ifDisable ? 2 : 1
      };
      let res = await XXXApi.operateDisabled(postData);
      if (res.code !== '0') {
        this.$message.error(res.errorMsg);
        return;
      }
      this.$message.success('操作成功！');
      this.searchList();
    },
    toDelete(item) {
      this.$confirm({
        title: '提示',
        content: '确定要删除该XX？',
        onOk: async () => {
          let postData = {

          };
          let res = await XXXApi.XXX(postData);
          if (res.code !== '0') {
            this.$message.error(res.errorMsg);
            this.loading = false;
            return;
          }
          this.$message.success('删除成功')
          //最后一页删除最后一条数据，自动改变页码
          const totalPage = Math.ceil((this.total - 1) / this.pageInfo.pageSize); // 总页数
          this.pageInfo.pageNum = this.pageInfo.pageNum > totalPage ? totalPage : this.pageInfo.pageNum;
          this.pageInfo.pageNum = this.pageInfo.pageNum < 1 ? 1 : this.pageInfo.pageNum;
          this.searchList();
        }
      })
    },
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
        exportExcel(this.searchFormVal, interfaceUrl, "XXX").then(() => {
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
