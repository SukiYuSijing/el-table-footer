<template>
  <table
          cellspacing="0"
          cellpadding="0"
          border="0"
          :style="{
      width: bodyWidth
    }"
  >
    <colgroup>
      <col v-for="column in columns" :key="column.id" :name="column.id" />
      <col v-if="hasGutter" name="gutter" />
    </colgroup>
    <tbody :class="{ 'has-gutter': hasGutter }">
    <tr v-for="(row, rowIndex) in data" :key="rowIndex">
      <template>
        <td  v-for="(column, cellIndex) in columns"
             :key="column.id"
             :class="getRowClasses(column, cellIndex)"
             :rowspan="spanList[rowIndex]&&spanList[rowIndex][cellIndex]&&spanList[rowIndex][cellIndex].rowSpan?spanList[rowIndex][cellIndex].rowSpan:1"
             :colspan="spanList[rowIndex]&&spanList[rowIndex][cellIndex]&&spanList[rowIndex][cellIndex].colSpan?spanList[rowIndex][cellIndex].colSpan:1"
             v-if="!hiddenCell.includes([rowIndex,cellIndex].join(','))">
          <render-cell  v-if="showColumn?cellIndex>=showColumn:true" :row="row" :column="column" :cellIndex="cellIndex" :cell="$parent.$scopedSlots[column.property]"></render-cell>
          <template v-else>
            <div v-for="(item,index) in lableValue">
              <div v-if="cellIndex==item">{{data[rowIndex][labelKeys[index]]}}</div>
            </div>
          </template>
        </td>
      </template>
      <th v-if="hasGutter" class="gutter"></th>
    </tr>
    </tbody>
  </table>
</template>

<script>
    import RenderCell from './render-cell'
    import { Table } from 'element-ui'
    import LayoutObserver from 'element-ui/packages/table/src/layout-observer'

    export default {
        components: {
            RenderCell
        },
        props: {
            data: {
                type: Array,
                default: () => ([])
            },
            fixed: String
        },
        data(){
            return {
                labelKeys:[],
                lableValue:[],
                formatData:[],
                spanList: {},
                hiddenCell:[],
                showColumn:0
            }
        },
        destroyed () {
            this.tableLayout.removeObserver(this)
        },
        methods: {
            init () {
                this.tableLayout.addObserver(this)
            },
            getSpanList(arr){
                this.labelKeys =[]
                this.lableValue = []
                var labelArr = {}
                let keys = Object.keys(arr[0]).filter(item=>item.startsWith("level")&&item.endsWith("Label"))
                keys.forEach(i=>labelArr[i]=[])
                var labelObj = arr[0].labelObj
                this.labelKeys = Object.keys(labelObj)
                this.lableValue = Object.values(labelObj).reduce((all,current)=>{
                    console.log(current)
                    all.push(all[all.length-1]?Number(current)+Number(all[all.length-1]):current)
                    return all
                },[0])
                this.labelCellIndex = labelArr
                arr.forEach((item,index)=>{
                    if(index==0) {
                        for(let i in labelArr){
                            labelArr[i].push(index)
                        }
                    }
                    if(index>0){
                        // debugger
                        var l = Object.keys(labelArr).length
                        for(var i=1;i<=l;i++){
                            if(item["level"+i+"Label"]!==arr[index-1]["level"+i+"Label"]){
                                for(let j =i;j<=l;j++){
                                    if(!labelArr["level"+j+"Label"].includes(index))
                                        labelArr["level"+j+"Label"].push(index)
                                }
                            }
                        }
                    }
                    if(index == arr.length-1){
                        for(let i in labelArr){
                            labelArr[i].push(index+1)
                        }
                    }
                })
                var row = 0
                var col = 0
                var obj = {}
                for(let i in labelArr){
                    row = 0
                    var value = labelArr[i]
                    for(let index in labelArr[i]){
                        if(index>=0&&index<value.length){
                            var rs = Number(value[Number(index)+1])-Number(value[index])
                            var cs = labelObj[i]
                        }
                        if(!value[index]&&value[index]!=0) break
                        row = value[index]
                        obj[row] = obj[row] || {}
                        obj[row][col] = {
                            rowSpan:rs,
                            colSpan:cs
                        }
                    }
                    col = col +Number(labelObj[i])
                }
                this.spanList = obj
                this.hiddenCell = []
                for(let i in obj){
                    for(let j in obj[i]){
                        let _cs = obj[i][j].colSpan
                        let _rs = obj[i][j].rowSpan
                        for(let i1 = Number(i);i1<Number(i)+_rs;i1++){
                            for(let j1 = Number(j);j1<Number(j)+_cs;j1++){
                                if(i!=i1||j!=j1){
                                    this.hiddenCell.push([i1,j1].join(","))
                                }

                            }
                        }
                    }
                }
                this.showColumn = 0
                let u = keys.length
                while (u>=1){
                    this.showColumn = this.showColumn +labelObj['level'+u+'Label']
                    u--
                }
            },
            ...LayoutObserver.methods,
            ...Table.components.TableFooter.methods
        },
        computed: {
            table () {
                return this.$parent.table
            },
            columns () {
                return (this.table && this.table.store.states.columns) || []
            },
            tableLayout () {
                return (this.table && this.table.layout) || {}
            },
            hasGutter () {
                return !this.fixed && this.tableLayout.gutterWidth
            },
            bodyWidth () {
                return this.table && this.table.layout.bodyWidth + 'px'
            }
        },
        watch:{
            data:{
                deep:true,
                immediate:true,
                handler(newVal){
                    console.log(newVal,Object.keys(newVal[0]).filter(item=>item.startsWith("level")&&item.endsWith("Label")).length)
                    if(Object.keys(newVal[0]).filter(item=>item.startsWith("level")&&item.endsWith("Label")).length > 0 ){
                        this.getSpanList(newVal)
                    }


                }
            }
        }
    }
</script>
