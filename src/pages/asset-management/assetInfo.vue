<template>
  <div class="assetInfo">

    <el-card style="margin-bottom: 10px">
      <div slot="header" class="clearfix">
        <el-page-header @back="goBack" content="协力大厦">
        </el-page-header>
      </div>
      <div>
        <InfoBox
          style="float: left; margin:0 40px 10px 15px"
          v-for="(item, index) in infoBoxData" :type='item.type'
          :key="'info' + index"
          :data="item"
        ></InfoBox>
        <div style="clear:both"></div>
      </div>

    </el-card>

    <el-card>
      <div>
        <el-select size="small" style="width: 150px" class="mr-10" v-model="requirement.area.value" placeholder="面积选择">
          <el-option
            v-for="item in requirement.area.areaList"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-select size="small" style="width: 150px" class="mr-10" v-model="requirement.state.value" placeholder="进退驻状态">
          <el-option
            v-for="item in requirement.state.stateList"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-select size="small" style="width: 150px" class="mr-10" v-model="requirement.timeLimit.value" placeholder="合同期限">
          <el-option
            v-for="item in requirement.timeLimit.timeLimitList"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-select size="small" style="width: 150px" class="mr-10" v-model="requirement.source.value" placeholder="招商类别">
          <el-option
            v-for="item in requirement.source.sourceList"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-select size="small" style="width: 150px" class="mr-10" v-model="requirement.empty.value" placeholder="空置状态">
          <el-option
            v-for="item in requirement.empty.emptyList"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-button
          size="small"
          type="primary"
          style="float: right"
          @click="() => this.addRoomShow = true"
          icon="el-icon-plus">
          新增房间
        </el-button>
      </div>
      <el-divider></el-divider>

<!--      状态-->
      <div style="margin-bottom: 10px;cursor: pointer">
        <div class="typeWrap" @click="handleStatusClick(item)" v-for="item in statusList" :key="item.str">
          <div class="status" :style="{background: item.color}"></div>
          <div class="text">{{item.str}}</div>
        </div>
        <div style="float: right">
          <el-button size="mini" @click="() => this.showType = !this.showType">切换</el-button>
        </div>
      </div>

<!--      楼宇列表-->

      <div class="list" :key="'list-' + index" v-for="(item, index) in fakerList">
        <div class="list-header">
          <div>{{fakerList.length - index}}楼</div>
          <div>1500㎡</div>
        </div>
        <div class="list-wrap">
          <div v-for="(subItem, subIndex) in item" :key="'listItem' + subIndex" >
            <div
              class="list-item"
              @click="roomInfo({index,subIndex})"
              :style="{
                width: !showType ? 'calc(' + 100 / item.length + '% - 5px)': 'calc(' + subItem.area * 100 / item[item.length - 1].allArea + '% - 5px)',
                background: subItem.isFind || !filterStatus ? subItem.bgColor : '#DCDCDC' }">
              <div class="text">阿里巴巴</div>
              <div class="sub-text" style="margin-bottom: 8px">{{subItem.area}}㎡</div>
              <div class="sub-text">2019-11-11到期</div>
              <div class="status">{{subItem.statusStr}}</div>
            </div>
          </div>

        </div>
        <div class="clear"></div>
      </div>

<!--      房间信息-->
       <el-drawer
        title="房间详情"
        custom-class="drawer-r"
        :visible.sync="roomInfoState"
        size="1186px"
        direction="rtl">
        <HeaderCard :data="roomInfo_header">
          <template #headerCardBtns>
          <div class="btnBox" v-for="(item,i) in roomInfo_header.button" :key="(item,i)" @click="open(item.name)">
            <i class="iconfont" v-html="item.icon"></i>
            <span class="headerCard-btn-name">{{item.name}}</span>
          </div>
        </template>
        </HeaderCard>
        <HeaderInfo type=1 :data="roomInfo_info"></HeaderInfo>
        <div class="drawer-body" style="height: 500px;">
          <BodyCard type=2 :data="roomInfo_body_table1">
            <template #btn>
              <el-button
                :style="{height: '80%',margin: 'auto 8px'}"
                size="mini"
              >新建合同</el-button>
            </template>
          </BodyCard>
          <BodyCard type=2 :data="roomInfo_body_table2">
            <!-- <template #btn>
              <el-button
                :style="{height: '80%',margin: 'auto 8px'}"
                icon="el-icon-plus"
                size="mini"
              >客户 </el-button>
            </template> -->
          </BodyCard>
        </div>
      </el-drawer>
    </el-card>

    <el-dialog
      title="添加房间"
      :visible.sync="addRoomShow"
      width="600px"
    >
      <div>
        <ParkForm
          ref="tt"
          :formList="$formsLabels.addRoomForm"
          :itemList="[]">
        </ParkForm>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="test(222)">确定</el-button>
      </span>

    </el-dialog>
  </div>
</template>

<script>
import ElDivider from 'element-ui/packages/divider/src/main'
import InfoBox from '@/components/InfoBox/index.vue'
export default {
  name: 'assetInfo',
  components: {
    ElDivider,
    InfoBox
  },
  data () {
    return {
      showType: true,
      options: [
        {
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }
      ],
      value: '',
      fakerList: [
      ],
      colorList: ['#57D1E2', '#46D2A8', '#F1A468', '#626C91', '#626C91'],
      statusList: [
        {
          color: '#57D1E2',
          code: 1,
          str: '在租'
        },
        {
          color: '#46D2A8',
          code: 2,
          str: '待招商'
        },
        {
          color: '#F1A468',
          code: 3,
          str: '自用'
        },
        {
          color: '#626C91',
          code: 4,
          str: '未分配'
        },
        {
          color: '#626C91',
          code: 5,
          str: '锁定'
        }
      ],
      infoBoxData: [
        {
          type: 0,
          title: {
            name: '管理面积',
            note: '测试文本'
          },
          value: {
            value: 20311400.3,
            unit: '㎡',
            chart: 0.24
          },
          subtitle: {
            name: '总房源数量',
            value: 22,
            unit: '间'
          }
        },
        {
          type: 'num',
          title: {
            name: '出租率',
            note: '测试文本'
          },
          value: {
            value: 55,
            unit: '%',
            chart: 0.24
          },
          subtitle: {
            name: '本月签约面积',
            value: 22,
            unit: '㎡'
          }
        },
        {
          type: 'num',
          title: {
            name: '在租实时均价',
            note: '测试文本'
          },
          value: {
            value: 2,
            unit: '元/㎡·天',
            chart: 0.24
          },
          subtitle: {
            name: '本月签约均价',
            value: 0,
            unit: '元/㎡·天'
          }
        },
        {
          type: 0,
          title: {
            name: '可招商面积',
            note: '测试文本'
          },
          value: {
            value: 20311400.3,
            unit: '㎡',
            chart: 0.24
          },
          subtitle: {
            name: '可招商房间',
            value: 22,
            unit: '间'
          }
        },
        {
          type: 'chart',
          title: {
            name: '当前计租率',
            note: '测试文本'
          },
          value: {
            value: 22.3,
            unit: '%',
            chart: 0.24
          },
          subtitle: {
            name: '预计全年计租率',
            value: 22,
            unit: '%'
          }
        }
      ],
      requirement: {
        area: {
          areaList: [
            {
              value: 1,
              label: '100m³以内'
            },
            {
              value: 2,
              label: '100-200m³'
            },
            {
              value: 3,
              label: '200-300m³'
            },
            {
              value: 4,
              label: '300m³以上'
            }
          ],
          value: ''
        },
        state: {
          stateList: [
            {
              value: 1,
              label: '进驻'
            },
            {
              value: 2,
              label: '退驻'
            }
          ],
          value: ''
        },
        timeLimit: {
          timeLimitList: [
            {
              value: 1,
              label: '半年以下'
            },
            {
              value: 2,
              label: '一年以内'
            },
            {
              value: 3,
              label: '三年以内'
            },
            {
              value: 4,
              label: '三年以上'
            }
          ],
          value: ''
        },
        industry: {
          industryList: [
            {
              value: 1,
              label: '金融'
            },
            {
              value: 2,
              label: '物流'
            },
            {
              value: 3,
              label: '电商'
            },
            {
              value: 4,
              label: '互联网'
            },
            {
              value: 0,
              label: '其他'
            }
          ],
          value: ''
        },
        source: {
          sourceList: [
            {
              value: 1,
              label: '中介'
            },
            {
              value: 2,
              label: '广告'
            },
            {
              value: 0,
              label: '其他'
            }
          ],
          value: ''
        },
        empty: {
          emptyList: [
            {
              value: 1,
              label: '占用'
            },
            {
              value: 2,
              label: '预定'
            },
            {
              value: 0,
              label: '待租'
            }
          ],
          value: ''
        }
      },
      roomInfoState: false, // 房间信息弹窗
      roomInfo_header: {
        title: '',
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
            name: '更多',
            icon: '&#xe86d;',
            function: 'click1'
          }
        ]
      },
      roomInfo_info: {
        label: [
          { prop: 'area', label: '面积(㎡)' },
          { prop: 'roomState', label: '房源状态' },
          { prop: 'state', label: '招商状态' },
          { prop: 'price', label: '预租单价(元/㎡·天)' },
          { prop: 'type', label: '房源类型' },
          { prop: 'decorate', label: '装修' },
          { prop: 'tag', label: '表签' }
        ],
        tableData: [{
          area: '360.00',
          roomState: '待招商',
          state: '可招商',
          price: '3.00',
          type: '办公',
          decorate: '简装',
          tag: '自带办公家居，随时入住'
        }]
      },
      roomInfo_body_table1: {
        title: '合同信息',
        info: {
          label: [
            { prop: 'tenant', label: '租户' },
            { prop: 'start', label: '计租日' },
            { prop: 'end', label: '结束日' },
            { prop: 'price', label: '合同单价' },
            { prop: 'state', label: '状态' }
          ],
          tableData: [
            // {
            //   tenant: '拓源科技',
            //   start: '2019/11/11',
            //   end: '2020/11/10',
            //   price: '3.00元/㎡·天',
            //   state: '新建待审核'
            // }
          ]
        }
      },
      roomInfo_body_table2: {
        title: '招商信息',
        info: {
          label: [
            { prop: 'tenant', label: '租客' },
            { prop: 'tel', label: '联系电话' },
            { prop: 'state', label: '客户状态' },
            { prop: 'date', label: '跟进人员' },
            { prop: 'channel', label: '招商渠道' },
            { prop: 'remarks', label: '备注' }
          ],
          tableData: [
          ]
        }
      },
      addRoomShow: false,
      // addRoomFormList: [
      //   {
      //     title: '房间信息',
      //     children: [
      //       {
      //         type: 'select',
      //         label: '所属楼宇',
      //         key: 'u',
      //         placeholder: '请选择',
      //         options: [
      //           {
      //             label: '协力大厦1',
      //             value: 's1'
      //           },
      //           {
      //             label: '协力大厦2',
      //             value: 's2'
      //           },
      //           {
      //             label: '协力大厦3',
      //             value: 's3'
      //           }
      //         ],
      //         rule: [
      //           { required: true, message: '请输入', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'select',
      //         label: '所属楼层',
      //         key: 'u2',
      //         placeholder: '请选择',
      //         options: [
      //           {
      //             label: '一楼',
      //             value: 's1'
      //           },
      //           {
      //             label: '二楼',
      //             value: 's2'
      //           },
      //           {
      //             label: '三楼',
      //             value: 's3'
      //           },
      //           {
      //             label: '四楼',
      //             value: 's4'
      //           },
      //           {
      //             label: '五楼',
      //             value: 's5'
      //           }
      //         ],
      //         rule: [
      //           { required: true, message: '请输入', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'input',
      //         label: '房间号',
      //         key: 'u3',
      //         placeholder: '请输入',
      //         rule: [
      //           { required: true, message: '该项为必填', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'input',
      //         label: '面积',
      //         key: 'u4',
      //         placeholder: '请输入',
      //         rule: [
      //           { required: true, message: '该项为必填', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'select',
      //         label: '状态',
      //         key: 'u5',
      //         placeholder: '请选择',
      //         options: [
      //           {
      //             label: '在租',
      //             value: 's1'
      //           },
      //           {
      //             label: '待招商',
      //             value: 's2'
      //           },
      //           {
      //             label: '自用',
      //             value: 's3'
      //           },
      //           {
      //             label: '锁定',
      //             value: 's4'
      //           },
      //           {
      //             label: '未分配',
      //             value: 's5'
      //           }
      //         ],
      //         rule: [
      //           { required: true, message: '请选择', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'input',
      //         label: '联系人',
      //         key: 'u6',
      //         placeholder: '请输入',
      //         rule: [
      //           { required: true, message: '该项为必填', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'input',
      //         label: '联系人电话',
      //         key: 'u7',
      //         placeholder: '请输入',
      //         rule: [
      //           { required: true, message: '该项为必填', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'textarea',
      //         label: '房间描述',
      //         key: 'u8',
      //         placeholder: '请输入',
      //         rule: [
      //           // { required: true, message: '请输入', trigger: 'blur' },
      //           // { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
      //         ]
      //       },
      //       {
      //         type: 'upload-img',
      //         label: '房间图片',
      //         key: 'u1',
      //         placeholder: '请输入'
      //         // rule: [
      //         //   { required: true, message: '请输入', trigger: 'blur' },
      //         //   { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
      //         // ]
      //       }
      //     ]
      //   }
      // ],
      filterStatus: false
    }
  },
  methods: {
    getState (value) {
      this.state = value
    },
    random (min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    roomInfo (id) {
      this.roomInfoState = true
      // console.log(id.index,id.subIndex);
      this.roomInfo_header.title = id.index + '楼00' + id.subIndex + '室'
    },
    open (i) {
      this.$message('这里是' + i)
    },
    goBack () {
      this.$router.go(-1) // 后退
    },
    handleStatusClick (data) {
      // if (this.filterStr !== data.str) {
      //   this.filterStatus = false
      // }
      this.filterStatus = !this.filterStatus
      this.findRoomByStatus(data.code)
    },
    findRoomByStatus (code) {
      this.fakerList.forEach(item => {
        item.forEach(sub => {
          sub.isFind = false
          if (sub.code === code) {
            sub.isFind = true
          }
        })
      })
    }
  },
  created () {
    let fakerList = []
    for (let i = 0; i < 6; i++) {
      let arr = []
      let allArea = 0
      let randomLength = this.random(3, 8)
      for (let j = 0; j < randomLength; j++) {
        let area = this.random(150, 300)
        allArea += area
        let status = this.random(0, 5) % 5
        arr.push({
          area,
          allArea,
          status,
          code: status + 1,
          checked: false,
          position: i + '-' + j,
          isFind: false,
          statusStr: this.statusList[status].str,
          bgColor: this.statusList[status].color
        })
      }
      fakerList.push(arr)
    }
    console.log(fakerList)
    this.fakerList = fakerList
    this.$https.post(this.$urls.park.get_list, { page_no: 1, page_size: 10 }).then((res) => {
      console.log(res)
    })
  }
}
</script>

<style lang="less" scoped>
@import '../../assets/style/index.less';
  *{
    box-sizing: border-box;
  }
  .assetInfo{
    width: 100%;
    height: 100%;
    position: relative;
    .box-card-title {
      position: relative;
      .back .el-icon-arrow-left{
        font-size: 32px;
        color: @blue;
        font-weight: 800;
        cursor: pointer;
      }
      .parkName{
        width: 230px;
        height: 100%;
        position: absolute;
        top: 0;
        left: 80px;
        i{
          font-size: 32px;
          color: @green;
          position: absolute;
          top: 25px;
        }
        p{
          width: ~"calc(100% - 50px)";
          position: absolute;
          right: 0;
          text-align: center;
        }
        .park-building{
          top: 10px;
          font-size: 18px;
        }
        .details{
          top: 50px;
          cursor: pointer;
          color: @blue;
        }
      }
    }

    .park,.building,.room{
      width: 100%;
      height: 100%;
    }
    .select{
      margin-right: 10px;
    }
    .typeWrap{
      display: inline-block;
      margin-bottom: 15px;
      .status{
        width: 16px;
        height: 16px;
        border-radius: 2px;
        vertical-align: middle;
        display: inline-block;
      }
      .text{
        margin: 0 40px 0 8px;
        display: inline-block;
        vertical-align: middle;
        font-size: 12px;
        color: rgba(0, 0, 0, 0.65);
      }
    }
    .list{
      width: 100%;
      margin-bottom: 8px;
      &-header{
        width: 80px;
        background-color: white;
        height: 80px;
        float: left;
        border-bottom: 1px solid #c6c6c8;
      }
      &-wrap{
        width: calc(~"100% - 80px");
        float: left;
        height: 80px;
        /*background-color: green;*/
        .list-item{
          float: left;
          width: 25%;
          height: 80px;
          padding: 13px 9px;
          color: white;
          position: relative;
          margin-right: 5px;
          .text{
            font-size: 14px;
            height: 14px;
            line-height: 14px;
            margin-bottom: 8px;
          }
          .text:last-child{
            margin-bottom: 0;
          }
          .sub-text{
            font-size: 12px;
            height: 12px;
            line-height: 12px;
          }
          .status{
            position: absolute;
            right: 0;
            top: 0;
            padding: 0 12px;
            height: 24px;
            background:rgba(255,255,255,0.45);
            color: #5E5E5E;
            line-height: 24px;
            text-align: center;
            font-size: 12px;
          }
        }
      }
    }
  }
  .clear{
    clear: both;
  }
</style>
