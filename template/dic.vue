<template>

</template>
<script>
export default {
  name: 'dic',
  methods: {
    //获取操作类型字典
    async findDicListByTypeAndKey(){
      let params={
        // dicType: "checkInType",
        // dicKey: "checkInType",
        // systemId: 'OTA'
      }
      // this.operateTypeList = await this.$dictUtil.getDicList(params)
    },
    async findDicListByTypeAndKey() {
      let res = await this.$dictUtil.getAllDicList({
        dicCodeList: ['hotel_info_extract_type', ],
        systemId: 'operation'
      })
      //酒店系列
      this.extractTypeList = this.$dictUtil.getDicListByCode(res, 'hotel_info_extract_type')
    }
  }
}
</script>



<style scoped lang="less">

</style>
