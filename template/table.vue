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
      <!-- 表格操作列按钮 -->
      <template slot="action" slot-scope="{record, text}">
        <template slot="action" slot-scope="{record, text}">
          <a-button size="small" type="link" @click="toEdit(record)">编辑</a-button>
          <a-button size="small" type="link" @click="toDetail(record)">详情</a-button>
          <a-button size="small" type="link" class="danger-btn" @click="toDelete(record)">删除</a-button>
        </template>
      </template>
    </ald-table>
  </div>

</template>

<script>
export default {
  name: 'table',
  data() {
    return {
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
    }
  }
}
</script>


<style scoped lang="less">

</style>
