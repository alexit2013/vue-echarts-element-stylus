<template>
  <div class="element-table-box" ref="tableBox">
    <el-table ref="elementtable" @sort-change="sortchange" tooltip-effect="dark" :data="tableArr" style="width: 100%" :height="tableHeight">  
      <el-table-column      
        v-for="(item,i) in MenuHeaderData"
        v-if="item.hidden ? false : true"
        :align="item.align ? item.align : 'center' "
        :width="item.width ? item.width : '200'"
        :fixed="i < 2"
        :label="item.label+item.type"
        :column-key="item.dataKeys[0]"
        :sortable="item.isSortable == false ? false : 'custom'"
        :prop="item.tip"
        :render-header="renderHeader"
        :key="i">
          <template slot-scope="scope">
            <slot :row="scope.row" :$index="i" :item="item">
              <!-- 当前列展示的数据，可以一个展示多个数据-->
              <span v-if="item.dataKeys.length">
                <span>{{ scope.row[item.dataKeys[0]] }}</span>
              </span>            
            </slot>   
          </template>
      </el-table-column>
    </el-table>    
  </div>
</template>

<script>


  export default {
    data() {
      return {
        tableArr: [],//表格数据
      }
    },
    props: ["tableData", "tableHeight", "MenuHeaderData"],
    watch: {
      tableData(val) {
        this.tableArr = [...val]
      }
    },

    methods: {
      // 渲染表头
      renderHeader(createElement, { column, $index}){
        let columnData = column;
        /**GLobalRenderHeader函数在common/js/renderHeader.js*/
        return this.GlobalRenderHeader(createElement, columnData);
      },

      // 自定义排序，因为element自带的排序对json中的数字解析为字符串，出错
      sortchange(row) {
         /**GLobalRenderHeader函数在common/js/globalSort.js*/
        this.tableArr = this.GlobalSort(row, 'upTime', this.tableData)
      },

    }
  }
</script>
<style lang="stylus">
.element-table-box
  margin 8px 16px 0 16px
  border 1px solid #e6ebf5
  box-sizing border-box
  .el-table
    box-sizing border-box
    .caret-wrapper
      position absolute
      top 60%
      transform translateY(-50%)
  .el-table .is-hidden .cell
    visibility visible
</style>