<template>
<div class="test">
<!--  eslint-disable-->
  <el-button type="primary" @click="drawer()">弹窗</el-button>
  <el-drawer
  title="房间详情"
  custom-class="drawer-r"
  :visible.sync="drawerState"
  size="1186px"
  direction="rtl">
    <HeaderCard :data="drawer_header"></HeaderCard>
    <HeaderInfo type=1 :data="drawer_info"></HeaderInfo>
    <div class="drawer-body" style="height:500px;">
      <BodyCard type=1 :data="drawer_body"></BodyCard>
      <BodyCard type=2 :data="drawer_body_table"></BodyCard>
      <BodyCard type=2 :data="drawer_body_table"></BodyCard>
    </div>
  </el-drawer>
  <el-divider></el-divider>

  <el-upload
    class="upload-demo"
    ref="upload"
    :action="url"
    :on-preview="handlePreview"
    :on-remove="handleRemove"
    :before-upload="handleBeforeUpload"
    :on-success="handleSuccess"
    :file-list="fileList"
  >
    <el-button size="small" type="primary">点击上传</el-button>
    <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
  </el-upload>
  <el-divider></el-divider>

  <el-card>
    <el-row :gutter="20">
      <el-col :span="6">
        <el-input v-model="API_input" placeholder="请输入API"></el-input>
        <el-input type="textarea"  rows="10" placeholder="请输入要传的参数" v-model="API_textarea2"></el-input>
        <el-button style="margin-bottom: 10px;" size="small"
        type="primary" @click="API_test">API测试</el-button>
      </el-col>
      <el-col :span="6">
        <el-input type="textarea" rows="15" placeholder="返回数据" v-model="API_textarea"></el-input>
      </el-col>
    </el-row>
  </el-card>
  <el-divider></el-divider>

  <el-card style="width: 800px">

    <div v-for="(a, ai) in testData">
      <h1>#{{a.index+ '.' +a.title}}</h1>
      <div v-for="(b, bi) in a.content" style="padding-left: 30px">
        <h2>·{{a.index}}-{{bi + 1}}{{b.title}}</h2>
        <div v-for="(c, ci) in b.content" style="padding-left: 30px">
          <h3>{{a.index}}-{{bi + 1}}-{{ ci + 1 }}{{c.title}}</h3>
          <div>api: {{c.api}}</div>
          <div>业务参数</div>
          <table border="1" cellpadding="0" cellspacing="0" width="600px">
            <tr><th>参数名称</th><th>参数类型</th><th>是否必须</th><th>参数描述</th></tr>
            <tr v-for="item in c.business" v-if="c.explain.length">
              <td>{{item.key}}</td>
              <td>{{item.type}}</td>
              <td>{{item.required}}</td>
              <td width="400px">{{item.description}}</td>
            </tr>
          </table>
          <br/>
          <div>返回参数</div>
          <table border="1" cellpadding="0" cellspacing="0" width="600px">
            <tr><th>参数名称</th><th>参数类型</th><th>是否必须</th><th>参数描述</th></tr>
            <tr v-for="item in c.return" v-if="c.explain.length">
              <td>{{item.key}}</td>
              <td>{{item.type}}</td>
              <td>{{item.required}}</td>
              <td width="400px">{{item.description}}</td>
            </tr>
          </table>
          <br/>
          <div v-if="c.explain.length">list说明</div>
          <table border="1" cellpadding="0" cellspacing="0" width="600px">
            <tr><th>参数名称</th><th>参数类型</th><th>是否必须</th><th>参数描述</th></tr>
            <tr v-for="item in c.explain" v-if="c.explain.length">
              <td>{{item.key}}</td>
              <td>{{item.type}}</td>
              <td>{{item.required}}</td>
              <td width="400px">{{item.description}}</td>
            </tr>
          </table>
          <br>
        </div>

      </div>

    </div>

  </el-card>

</div>
</template>

<script>
import HeaderCard from '@/components/HeaderCard/index.vue'
import HeaderInfo from '@/components/HeaderInfo/index.vue'
import BodyCard from '@/components/BodyCard/index.vue'
export default {
  name: 'test',
  components: {
    HeaderCard, HeaderInfo, BodyCard
  },
  props: [''],
  data () {
    return {
      API_input: '',
      API_textarea: '',
      API_textarea2: '',
      url: 'http://192.168.0.231:3000/upload',
      fileList: [],
      drawerState: false,
      drawer_header: {
        icon: '&#xe60c;',
        title: '西港发展中心  /  B栋 /  301、302',
        button: [
          { name: '按钮', icon: '&#xe62a;', function: 'click1' },
          { name: '按钮', icon: '&#xe62a;', function: 'click2' },
          { name: '按钮', icon: '&#xe64f;', function: 'click3' },
          { name: '按钮', icon: '&#xe607;', function: 'click4' },
          { name: '按钮', icon: '&#xe60c;', function: 'click3' },
          { name: '按钮', icon: '&#xe60c;', function: 'click4' }
        ]
      },
      drawer_info: {
        label: [
          { prop: 'date', label: '日期' },
          { prop: 'name', label: '姓名' },
          { prop: 'address', label: '地址' }
        ],
        tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }]
      },
      drawer_body: {
        title: '房间信息',
        info: [
          { name: '用户姓名', value: '付晓晓' },
          { name: '联系方式', value: '15648954632' },
          { name: '身份证', value: '300000000000000000' },
          { name: '地址', value: '浙江省杭州市西湖区黄姑山路工专路交叉路口' },
          { name: '用户姓名', value: '付晓晓', tag: 1 },
          { name: '用户姓名', value: '付晓晓' },
          { name: '用户姓名', value: '付晓晓' }

        ]
      },
      drawer_body_table: {
        title: '房间信息',
        info: {
          label: [
            { prop: 'date', label: '日期' },
            { prop: 'name', label: '姓名' },
            { prop: 'address', label: '地址' }
          ],
          tableData: [
            {
              date: '2016-05-02',
              name: '王小虎',
              address: '上海市普陀区金沙江路 1518 弄'
            },
            {
              date: '2016-05-02',
              name: '王小虎',
              address: '上海市普陀区金沙江路 1518 弄'
            }, {
              date: '2016-05-02',
              name: '王小虎',
              address: '上海市普陀区金沙江路 1518 弄'
            }, {
              date: '2016-05-02',
              name: '王小虎',
              address: '上海市普陀区金沙江路 1518 弄'
            }
          ]
        }
      },
      tableData: [{
        name: '2016-05-02',
        key: '王小虎',
        detail: '上海市普陀区金沙江路 1518 弄',
        type: 'int',
        require: false
      }],
      testData: []
    }
  },
  mounted () {
    for (let i = 0; i < 10; i++) {
      this.tableData.push({
        name: 'cc',
        key: 'cc',
        detail: '房产性质：0-其他、1-商业用房、2-生产用房、3-住宅',
        type: 'int',
        require: false
      })
    }
    this.testData = [
      {
        index: 3,
        title: '交互协议设计',
        content: [
          {
            title: '运营模块管理',
            content: [
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              }
            ]
          },
          {
            title: '运营模块管理',
            content: [
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              }
            ]
          },
          {
            title: '运营模块管理',
            content: [
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              }
            ]
          }
        ]
      },
      {
        index: 4,
        title: '交互协议设计',
        content: [
          {
            title: '运营模块管理',
            content: [
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              }
            ]
          },
          {
            title: '运营模块管理',
            content: [
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              }
            ]
          },
          {
            title: '运营模块管理',
            content: [
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              },
              {
                title: '添加园区模块',
                api: 'assets.park.add',
                business: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  },
                  {
                    key: 'address',
                    type: 'string',
                    required: true,
                    description: '园区地址'
                  },
                  {
                    key: 'estate_property',
                    type: 'string',
                    required: true,
                    description: `房产性质：0-其他、1-商业用房、2-生产用房、3-住宅;用字符串存储,逗号分隔,示例:"1,2,3"`
                  }
                ],
                return: [
                  {
                    key: 'list',
                    type: 'json array',
                    required: true,
                    description: '园区模块列表'
                  }
                ],
                explain: [
                  {
                    key: 'domain_id',
                    type: 'int',
                    required: true,
                    description: '域关联ID'
                  }
                ]
              }
            ]
          }
        ]
      }
    ]
  },
  watch: {
  },
  methods: {
    // API接口测试
    API_test () {
      let api = this.$urls.park.add
      let params2 = {
        // domain_id: 317,
        address: '协力大厦',
        estate_property: '1',
        usage: '1',
        complete_ts: '2019-11-07T04:55:20.176Z',
        acquire_way: 1,
        capital: 1,
        detail: '1',
        attached: {},
        memo: '1',
        name: '西岗2',
        built_area: 10000,
        total_invest: 8000,
        property: '',
        contacter: '',
        contact: '',
        state: 1,
        cover_area: 12000,
        actual_invest: 9000

      }
      let params = {
        page_size: 999,
        page_no: 1
      }
      let params3 = {
        domain_id: 464
      }
      let params4 = {
        pid: 454,
        name: '测试房间1',
        info: {},
        code: 111,
        domain_memo: '',
        room_property: 1,
        is_rentable: false,
        state: 1,
        room_usage: 0,
        decoration_standard: 0,
        area: 300,
        direction: 2,
        estate_property: '1',
        usage: '1',
        acquire_way: '1',
        acquire_ts: '2019-11-07T04:55:20.176Z',
        is_flue: false,
        floor_height: 10,
        bearing: 10000,
        attached: {},
        memo: ''

      }

      if (this.API_input) {
        api = this.API_input
      }
      if (this.API_textarea2) {
        params = JSON.parse(this.API_textarea2)
      }

      this.$https.post(api, params2).then((res) => {
        this.$message(res.msg)
        this.API_textarea = JSON.stringify(res, null, 2)
        if (res.code !== 1000) {
          this.API_textarea = JSON.stringify({
            err: res.err,
            msg: res.msg,
            track: res.track
          })
        }
        // console.log(this.API_textarea)
      })
    },
    showfile () {
      console.log(this.fileList)
    },
    drawer () {
      this.drawerState = true
    },
    submitUpload () {
      // this.$refs.upload.submit();
      this.$https.post('http://192.168.0.231:3000/upload', {}).then((res) => {
        // console.log(res)
      })
    },
    handleRemove (file, fileList) {
      console.log(file, fileList)
    },
    handlePreview (file) {
      console.log(file)
    },
    handleBeforeUpload (file) {
      // console.log(file, 'handleBeforeUpload')
      return true
    },
    handleSuccess (response, file, fileList) {
      // console.log(response, file, fileList)
    }
  }
}
</script>

<style lang="less" scoped>
@import '../../assets/style/index.less';
  h1{
    font-size: 40px;
    line-height: 60px;
  }
  h2{
    font-size: 30px;
    line-height: 40px;
  }
  h3{
    font-size: 20px;
    line-height: 30px;
  }
</style>
