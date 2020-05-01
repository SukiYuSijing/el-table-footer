# el-table-footer

一个用来实现 el-table 包含合计与总计或者其他场景的组件

![image](./demo.png)

[点击查看在线demo](https://code-farmer-i.github.io/el-table-footer/dist/)

## Install
```shell
npm install el-table-footer -S
```

## webpack.base.conf.js
因代码中使用了es6的语法 所以需要添加babel配置
``` javascript
'use strict'
const path = require('path')
const utils = require('./utils')
const config = require('../config')
const vueLoaderConfig = require('./vue-loader.conf')
// 此处为添加的配置
const fs = require('fs')

function resolve (dir) {
  return path.join(__dirname, '..', dir)
}

//此处为添加的配置
let dirsName = fs.readdirSync(resolve('node_modules')).filter(dirName => /el-table-footer/.test(dirName))
const includesDirs = dirsName.map(dir => resolve(`node_modules/${dir}/src`))

module.exports = {
  ... //省略的代码
  module: {
    rules: [
      // 省略代码...
      // 此处有添加babel-loader配置 ‘...includesDirs‘
      {
        test: /.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test'), resolve('node_modules/webpack-dev-server/client'), ...includesDirs]
      },
      {
        test: /.(png|jpe?g|gif|svg)(?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
```

## Quick Start
``` javascript
import Vue from 'vue'
import ElTableFooter from 'el-table-footer'

Vue.use(ElTableFooter)
```

### 基础用法
```html
<template>
  <div>
    <el-table
      :data="tableData"
      border
      ref="table"
    >
      <el-table-column
        prop="id"
        label="ID"
        width="400"
        fixed>
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="400">
      </el-table-column>
      <el-table-column
        prop="amount1"
        label="数值 1"
        width="400">
      </el-table-column>
      <el-table-column
        prop="amount2"
        label="数值 2"
        width="400">
      </el-table-column>
      <el-table-column
        prop="amount3"
        label="数值 3"
        width="400"
        fixed="right">
      </el-table-column>
    </el-table>
    <el-table-footer :data="footerData" ref="tableFooter"></el-table-footer>
  </div>
</template>

<script>
  export default {
    computed: {
      footerData () {
        return [this.summary1, this.summary2]
      }
    },
    data () {
      return {
        tableData: [{
          id: '12987122',
          name: '王小虎',
          amount1: '234',
          amount2: '3.2',
          amount3: 10
        }, {
          id: '12987123',
          name: '王小虎',
          amount1: '165',
          amount2: '4.43',
          amount3: 12
        }, {
          id: '12987124',
          name: '王小虎',
          amount1: '324',
          amount2: '1.9',
          amount3: 9
        }, {
          id: '12987125',
          name: '王小虎',
          amount1: '621',
          amount2: '2.2',
          amount3: 17
        }, {
          id: '12987126',
          name: '王小虎',
          amount1: '539',
          amount2: '4.1',
          amount3: 15
        }],
        summary1: {
          label: '合计',
          data: {
            amount1: 13414,
            amount2: 13414,
            amount3: 13414
          }
        },
        summary2: {
          label: '总计',
          data: {
            amount1: 13414,
            amount2: 13414,
            amount3: 13414
          }
        }
      }
    },
    mounted () {
      const {
        tableFooter,
        table
      } = this.$refs

      // 调用init方法传入表格实例初始化footer
      tableFooter.init(table)
    }
  }
</script>
```
### 如果需要合并单元格 
<template>
  <div>
    <el-table
            :data="tableData"
            border
            ref="table"
    >
      <el-table-column
              prop="id"
              label="ID"
              width="200"
              fixed>
      </el-table-column>
      <el-table-column
              prop="job"
              label="职业"
              width="200">
      </el-table-column>
      <el-table-column
              prop="name"
              label="姓名"
              width="200">
      </el-table-column>
      <el-table-column
              prop="name"
              label="姓名1"
              width="200">
      </el-table-column>
      <el-table-column
              prop="name"
              label="姓名2"
              width="200">
      </el-table-column>
      <el-table-column
              prop="age"
              label="年纪"
              width="200">
      </el-table-column>

          <el-table-column
                  prop="gender"
                  label="性别"
                  width="200">
          </el-table-column>
      <el-table-column
              prop="amount1"
              label="数值 1"
              width="200">
      </el-table-column>
      <el-table-column
              prop="amount2"
              label="数值 2"
              width="200">
      </el-table-column>
      <el-table-column
              prop="amount3"
              label="数值 3"
              width="200"
              fixed="right">
      </el-table-column>
    </el-table>
    <el-table-footer :data="footerData" ref="tableFooter"></el-table-footer>
  </div>
</template>
<script>
    export default {
        computed: {
            footerData () {
                return [this.summary1, this.summary2,this.summary3, this.summary4,this.summary5,
                    this.summary6, this.summary7,this.summary8, this.summary9
                ]
            }
        },
        data () {
            return {
                tableData: [{
                    id: '12987122',
                    name: '王小虎',
                    age:16,
                    job:"coder",
                    gender:"female",
                    amount1: '234',
                    amount2: '3.2',
                    amount3: 10
                }, {
                    id: '12987123',
                    name: '王小虎',
                    age:16,
                    job:"coder",
                    gender:"female",
                    amount1: '165',
                    amount2: '4.43',
                    amount3: 12
                }, {
                    id: '12987124',
                    name: '王小虎',
                    age:16,
                    job:"coder",
                    gender:"female",
                    amount1: '324',
                    amount2: '1.9',
                    amount3: 9
                }, {
                    id: '12987125',
                    name: '王小虎',
                    age:16,
                    job:"coder",
                    gender:"female",
                    amount1: '621',
                    amount2: '2.2',
                    amount3: 17
                }, {
                    id: '12987126',
                    name: '王小虎',
                    age:16,
                    job:"coder",
                    gender:"female",
                    amount1: '539',
                    amount2: '4.1',
                    amount3: 15
                }],
                summary1: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎1",
                    level2Label: '小计',
                    level3Label:"sub1",
                    level4Label:"subbbb",
                    data: {
                        amount1: 13414,
                        amount2: 13414,
                        amount3: 13414
                    }
                },
                summary2: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎1",
                    level2Label: '合计',
                    level3Label:"sub1",
                    level4Label:"subbbb",
                    data: {
                        amount1: 13414,
                        amount2: 13414,
                        amount3: 13414
                    }
                    },
                    summary3: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎2",
                        level2Label: '小计',
                        level3Label:"sub1",
                        level4Label:"subbbb",
                        data: {
                            amount1: 13414,
                            amount2: 13414,
                            amount3: 13414
                        }
                    },
                    summary4: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎2",
                        level2Label: '小计',
                        level3Label:"sub2",
                        level4Label:"subbbb",
                        data: {
                            amount1: 13414,
                            amount2: 13414,
                            amount3: 13414
                        }
                    },
                    summary5: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                        level1Label:"王小虎2",
                        level2Label: '合计',
                        level3Label:"sub2",
                        level4Label:"subbbb",
                        data: {
                            amount1: 13414,
                            amount2: 13414,
                            amount3: 13414
                        }
                },
                summary6: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎3",
                    level2Label: '合计1' +
                    '',
                    level3Label:"sub1",
                    level4Label:"subbbb",
                    data: {
                        amount1: 13414,
                        amount2: 13414,
                        amount3: 13414
                    }
                },
                summary7: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎3",
                    level2Label: '合计',
                    level3Label:"sub1",
                    level4Label:"subbbb",
                    data: {
                        amount1: 13414,
                        amount2: 13414,
                        amount3: 13414
                    }
                },
                summary8: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎3",
                    level2Label: '合计',
                    level3Label:"sub2",
                    level4Label:"subbbb",
                    data: {
                        amount1: 13414,
                        amount2: 13414,
                        amount3: 13414
                    }
                },
                summary9: {
                    labelObj:{
                        level1Label:2,
                        level2Label:2,
                        level3Label:2,
                        level4Label:1
                    },
                    level1Label:"王小虎3",
                    level2Label: '合计',
                    level3Label:"sub2",
                    level4Label:"subbbb",
                    data: {
                        amount1: 13414,
                        amount2: 13414,
                        amount3: 13414
                    }
                }

        }},
        mounted () {
            const {
                tableFooter,
                table
            } = this.$refs

            // 调用init方法传入表格实例初始化footer
            tableFooter.init(table)
        }
    }
</script>

## API

### El-Table-Editabled Props:

属性  |  说明  |  类型  |  默认值
:-------: | -------  |  :-------:  |  :-------:
data  |  footer数据  |  Array  |  --


### El-Table-Editabled Methods:

方法  |  说明  |  参数
:-------: | -------  |  :-------:
init  |  初始化footer(必须传入表格实例)  |  Function(tableInstance)

作者wx: ckang1229

