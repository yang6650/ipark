<template>
  <div>
    <el-card style="width: 100%">
      <span style="font-size: 16px;color: #303133;border-left: 3px solid #358dd8;padding-left: 10px">保洁工单</span>

      <el-select  size="small"
                  style="margin-left: 15px"
                  v-model="value2" placeholder="工单状态">
        <el-option
          v-for="item in options2"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>

      <el-input
        placeholder="搜索工单"
        size="small"
        style="width: 220px; margin-left: 15px"
        prefix-icon="el-icon-search"
        v-model="value">
      </el-input>

      <el-button
        style="float: right"
        type="primary"
        icon="el-icon-plus"
        size="small"
        @click="handleAddContract"
      >新建工单模板</el-button>
    </el-card>
    <el-card>
      <el-table
        :data="tableData"
        @row-click="workOrderState"
        style="width: 100%">
        <el-table-column
          prop="a"
          label="模板编号">
        </el-table-column>
        <el-table-column
          prop="b"
          label="模板名称">
        </el-table-column>
        <el-table-column
          prop="c"
          label="模板类型">
        </el-table-column>
        <el-table-column
          prop="d"
          label="状态">
        </el-table-column>
        <el-table-column
          prop="e"
          label="模板描述">
        </el-table-column>
      </el-table>
      <div style="width: 100%; text-align: right; padding-top: 20px">
        <el-pagination layout="prev, pager, next" :total="1000"> </el-pagination>
      </div>
    </el-card>

    <el-dialog
      title="新建工单模板"
      :visible.sync="addContractVisible"
      width="800px"
      :before-close="handleClose">
      <div>
        <ParkForm :formList="addContractFormList" :itemList="[]"></ParkForm>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
<!--      工单详情-->
      <el-drawer
      title="工单详情"
      custom-class="drawer-r"
      :visible.sync="workOrderInfoState"
      size="1186px"
      direction="rtl">
      <HeaderCard :data="workOrderInfo_header"></HeaderCard>
      <!-- <HeaderInfo type=1 :data="workOrderInfo_info"></HeaderInfo> -->
      <div class="drawer-body" style="height: 700px;">
        <BodyCard type=1 :data="workOrderInfo_body1"></BodyCard>
        <BodyCard type=1 :data="workOrderInfo_body2"></BodyCard>
        <BodyCard type=3 :data="workOrderInfo_body3"></BodyCard>
      </div>
    </el-drawer>
  </div>
</template>

<script>
import ParkForm from '@/components/ParkForm/index.vue'
import ElCard from 'element-ui/packages/card/src/main'
export default {
  name: 'index',
  components: {
    ElCard,
    ParkForm
  },
  data () {
    return {
      tableData: [],
      activeName: 'first',
      yearList: [
      ],
      options: [
        {
          value: '选项1',
          label: '全部'
        }, {
          value: '选项2',
          label: '维修'
        }, {
          value: '选项3',
          label: '保洁'
        }, {
          value: '选项4',
          label: '报事'
        }, {
          value: '选项5',
          label: '投诉'
        }],
      options2: [{
        value: '选项1',
        label: '全部'
      }, {
        value: '选项2',
        label: '已解决'
      }, {
        value: '选项3',
        label: '待解决'
      }
      ],
      value: '',
      value2: '',
      addContractVisible: false,
      addContractFormList: [
        {
          title: '租客信息',
          span: 12,
          minHeight: 350,
          children: [
            {
              type: 'input',
              label: '租客名称',
              key: 'i',
              placeholder: '请输入租客名称',
              rule: [
                { required: true, message: '请输入租客名称', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            }
          ]
        }, {
          title: '工单信息',
          span: 12,
          minHeight: 350,
          children: [
            {
              type: 'input',
              label: '服务地点',
              key: 'tenantName',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请输入合同编号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            },
            {
              type: 'input',
              label: '发起人',
              key: 'tenantName',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请输入合同编号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            },
            {
              type: 'select',
              label: '工单类型',
              key: 'tamplate',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请选择', trigger: 'change' }
              ],
              options: [
                {
                  label: '美食',
                  value: 's1'
                }, {
                  label: '美食美食',
                  value: 's2'
                }
              ]
            },
            {
              type: 'input',
              label: '联系电话',
              key: 'tenantName',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请输入合同编号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            },
            {
              type: 'date-picker',
              label: '预约服务时间',
              key: 'fr',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请输入合同编号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            }
          ]
        },
        {
          title: '工单摘要',
          span: 12,
          minHeight: 250,
          children: [
            {
              type: 'textarea',
              label: '摘要内容',
              key: 'fr',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请输入合同编号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            },
            {
              type: 'upload',
              label: '封面图片',
              key: 'ccc',
              placeholder: '请输入',
              rule: [
                { required: true, message: '请输入合同编号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
              ]
            }
          ]
        },
        {
          title: '房源信息',
          span: 12,
          minHeight: 250,
          children: [
            {
              type: 'cascader',
              label: '房源信息',
              key: 'fangyxx',
              rule: [
                { required: true, message: '请选择', trigger: 'change' }
              ],
              options: [{
                value: 1,
                label: '梦想小镇',
                children: [{
                  value: 2,
                  label: '1幢',
                  children: [
                    { value: 3, label: '101' },
                    { value: 4, label: '201' },
                    { value: 5, label: '205' }
                  ]
                }, {
                  value: 7,
                  label: '3幢',
                  children: [
                    { value: 8, label: '101' },
                    { value: 9, label: '103' },
                    { value: 10, label: '503' }
                  ]
                }, {
                  value: 12,
                  label: '8幢',
                  children: [
                    { value: 13, label: '202' },
                    { value: 14, label: '503' },
                    { value: 15, label: '603' }
                  ]
                }]
              }, {
                value: 17,
                label: '人工智能小镇',
                children: [{
                  value: 18,
                  label: '16幢',
                  children: [
                    { value: 19, label: '501' },
                    { value: 20, label: '505' }
                  ]
                }, {
                  value: 21,
                  label: '19幢',
                  children: [
                    { value: 22, label: '103' },
                    { value: 23, label: '105' }
                  ]
                }]
              }]
            }
          ]
        }
      ],
      tamplateFormList: [
        {
          type: 'select',
          label: '模板类型',
          key: 'tamplate',
          placeholder: '请输入',
          rule: [
            { required: true, message: '请选择', trigger: 'change' }
          ],
          options: [
            {
              label: '美食',
              value: 's1'
            }, {
              label: '美食美食',
              value: 's2'
            }
          ]
        }, {
          type: 'input',
          label: '模板名称',
          key: 'i',
          placeholder: '请输入',
          rule: [
            { required: true, message: '请输入模板名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ]
        }, {
          type: 'textarea',
          label: '模板描述',
          key: 'i11',
          placeholder: '请输入模板描述',
          rule: [
            { required: true, message: '请输入模板描述', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ]
        }, {
          type: 'upload',
          label: '模板描述',
          key: 'i11',
          placeholder: '请输入模板描述',
          rule: [
            { required: true, message: '请输入模板描述', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ]
        }
      ],
      workOrderInfoState: false,
      workOrderInfo_header: {
        title: '维修工单',
        button: [
          {
            name: '编辑',
            icon: '&#xe62a;',
            function: 'click1'
          },
          {
            name: '附件',
            icon: '&#xe655;',
            function: 'click1'
          },
          {
            name: '备注',
            icon: '&#xe7d1;',
            function: 'click1'
          },
          {
            name: '删除',
            icon: '&#xe64a;',
            function: 'click1'
          }
        ],
        data: {}
      },
      workOrderInfo_body1: {
        title: '工单信息',
        info: [
          { name: '工单类型', value: '维修工单' },
          { name: '发起人', value: '王丽华' },
          { name: '工单号', value: '5465489518XX' },
          { name: '工单摘要', value: '门窗维修' },
          { name: '工单状态', value: '已维修' },
          { name: '预约时间', value: '2019-10-20' }
        ]
      },
      workOrderInfo_body2: {
        title: '所属资产',
        info: [
          { name: '楼宇名称', value: 'XXX1' },
          { name: '楼层', value: '三楼' },
          { name: '房间号', value: 'A304' }
        ]
      },
      workOrderInfo_body3: {
        title: '工单评价',
        info: '无'
      }

    }
  },
  methods: {
    handleAddContract () {
      this.addContractVisible = true
    },
    workOrderState () {
      this.workOrderInfoState = true
    }
  },
  created () {
    [1, 2, 3, 4, 5, 6, 7, 8].forEach(item => {
      this.tableData.push(
        {
          a: 'xxx-xx-' + item,
          b: '出租合同模板' + item,
          c: '销售类' + item,
          d: item % 2 === 0 ? '启用' : '停用',
          e: '这是销售类合同的描述xxx'
        }
      )
    })
    // console.log(this.yearList)
  }
}
</script>

<style lang="less" scoped>
@import '../../assets/style/index.less';
  .el-card{
    margin-bottom: 20px;
  }
</style>
