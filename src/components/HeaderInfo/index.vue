<template>
<div class="headerInfo">
  <el-card class="box-card" shadow="never">
    <el-table v-if="type==1" :data="tableData" style="width: 100%">
      <el-table-column
        v-for="(item,i) in label"
        :key="(item,i)"
        :prop="item.prop"
        :label="item.label"
        :width="1186/data.label.length">
      </el-table-column>
    </el-table>
  </el-card>
</div>
</template>

<script>
export default {
  name: 'HeaderInfo',
  components: {
  },
  props: [
    'type', 'data'
  ],
  data () {
    return {
      label: this.data.label,
      tableData: this.data.tableData
    }
  },
  methods: {
  }
}
</script>

<style lang="less" scoped>
@import '../../assets/style/index.less';

.headerInfo{

    border-top: 2px solid #B2E0F4;
  /deep/ .box-card{
    height: 99px;
    border-bottom: none;
    .el-card__body{
      padding: 0;
      position: relative;
    }
  }
  /deep/ .el-table{
    &::before{
      height: 0;
    }
    .el-table__header-wrapper{
      th{

        border-bottom: none;
        color: @text-color;
        font-size: 14px;
        text-align: center;
      }
    }
    .el-table__body-wrapper{
      overflow: hidden;
      td{
        border-bottom: none;
        color: @blue;
        font-size: 18px;
        text-align: center;
      }
    }
    .cell{
      .ellipsis;
    }
  }
}
</style>
