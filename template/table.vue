<template>
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
                ;(pageInfo.pageNum = page), ( pageInfo.pageSize = pageSize), this.getHotelList()
              },
              onShowSizeChange: (current, size) => {
                ;(pageInfo.pageNum = current), (pageInfo.pageSize = size), this.getHotelList()
              },
            }"
    >
      <!-- 表格上方按钮 -->
      <template slot="actions">
        <a-button @click="toAdd()" type="primary">XXX</a-button>
      </template>
      <template slot="fullName" slot-scope="{text}">
        <span @click="toXXX(text)" style="cursor:pointer;color:#1890ff">{{text.fullName}}</span>
      </template>
      <template slot="createTime" slot-scope="{text, record}">
        <div>{{ record.createUserName }}</div>
        <div>{{ record.createTime }}</div>
      </template>
      <template slot="updateTime" slot-scope="{text, record}">
        <div>{{ record.updateUserName }}</div>
        <div>{{ record.updateTime }}</div>
      </template>
      <!-- 表格操作列按钮 -->
      <template slot="action" slot-scope="{text, record}">
        <a-button size="small" type="link" @click="toEdit(record)">编辑</a-button>
        <a-button size="small" type="link" @click="toDetail(record)">详情</a-button>
        <a-button size="small" type="link" class="danger-btn" @click="toDelete(record)">删除</a-button>
      </template>
    </ald-table>
  </div>

</template>

<script>
export default {
  name: 'table',
  data() {
    return {
      loading: false,
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
          title: '差评率',
          dataIndex: 'badEvaluateRate',
          customRender: (text, record) => { return `${this.$calcUtil.multiply(record.badEvaluateRate,100, 2)}%`}
        },
        {
          title: '入住类型',
          dataIndex: 'checkInType',
          customRender: text => this.$dictUtil.getValueByKey(this.checkInTypeList, text),
          width: 150
        },
        {
          title: '创建时间',
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
          title: '操作时间',
          dataIndex: 'createTime',
          customRender:  text => text ? momentUtil.standardDateTime(text) :'-',
          width: 120
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
    toEdit(item) {},
    toDetail(item) {},
    toOperate(item, ifDisable) {
      this.$confirm({
        title: '提示',
        content: ifDisable ? '确定要下架该活动？' : '确定要上架该活动？',
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
    // 禁用启用按钮事件
    toOperate(item) {
      this.$confirm({
        title: '提示',
        content: item.status ? '确定要启用礼品柜？' : '确定要禁用礼品柜？',
        okText: '确定',
        cancelText: '取消',
        onOk: async() => {
          await this.operateDisabled(item);
        }
      });
    },
    // 调接口禁用启用
    async operateDisabled(item) {
      let postData = {
        templateProjectId: item.templateProjectId
      }
      let res = await projectMissionApi.disableProjectTemplate(postData)
      if (res.code !== '0') {
        this.$message.error(res.errorMsg);
        item.loading = false;
        return;
      }
      item.loading = false;
      this.$message.success(item.enableState === 0 ? '启用成功' : '禁用成功');
      await this.XXXList();
    },
  }
}
</script>


<style scoped lang="less">

</style>
