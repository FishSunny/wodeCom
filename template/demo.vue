<template>
  <a-select-option v-for="item in orgGegionalList" :key="item.regionId" :value="item.regionId">
    {{item.name}}
  </a-select-option>
</template>

<script>
import { getCommonAreaList, getCompanyList, getOrganizeUserList } from '@/utils/commonData'
export default {
  name: 'demo',
  data() {

  },
  methods: {

  },
  created() {
    this.orgGegionalList = await getCommonAreaList({}); // 1获取区域列表

    // 2改变区域获取经销商列表
    this.dealerList = await getCompanyList({regionId: this.searchForm.orgRegionalId});

    // 3改变经销商获取人员列表
    this.salesmanList = await getOrganizeUserList({companyId: this.searchForm.orgDealerId});
  }
}
</script>



<style scoped lang="less">

</style>

