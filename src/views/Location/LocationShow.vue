<template>
  <div>
    <el-table
      ref="multipleTable"
      :data="tableData"
      border
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column prop="Id" label="序号"> </el-table-column>
      <el-table-column prop="StorageCode" label="库位编码"> </el-table-column>
      <el-table-column prop="StorageName" label="库位名称"> </el-table-column>
      <el-table-column prop="Area" label="面积"> </el-table-column>
      <el-table-column prop="WeightMAX" label="最大载重量"> </el-table-column>
      <el-table-column prop="Storage_X" label="库位位置X"> </el-table-column>
      <el-table-column prop="Storage_Y" label="库位位置Y"> </el-table-column>
      <el-table-column prop="Storage_Z" label="库位位置Z"> </el-table-column>
      <el-table-column prop="RepositoryName" label="所属仓库"> </el-table-column>
      <el-table-column prop="StorageBin" label="所属库位"> </el-table-column>
      <el-table-column prop="State" label="状态"> 
        <template slot-scope="scope">
          <span v-if="scope.row.State == true">启用</span>
          <span v-if="scope.row.State == false">未启用</span>
        </template>
      </el-table-column>
      <el-table-column prop="Remark" label="备注"> </el-table-column>
      <el-table-column
      fixed="right"
      label="操作"
      width="100">
      <template>
        <el-button type="text" size="small">查看</el-button>
        <el-button type="text" size="small">编辑</el-button>
      </template>
    </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pageindex"
      :page-sizes="[2, 3, 4]"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
  </div>
</template>
  
  <script>
export default {
  data() {
    return {
      tableData: [],
      multipleSelection: [],
      pagesize:3,
      pageindex:1,
      total:0
    };
  },
  
  created(){
    this.GetShow();
  },
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    //列表显示
    GetShow(){
      this.axios.get(this.$Url+"api/SAPWM/LocationShow", {
          params: {
            pagesize: this.pagesize,
            pageindex: this.pageindex,
          },
        }).then((res)=>{
        this.tableData=res.data.Plist;
        this.total = res.data.Fcount;
      })
    },
    handleSizeChange(val) {
        this.pagesize=val;
        this.GetShow();
      },
      handleCurrentChange(val) {
        this.pageindex=val;
        this.GetShow();
      },
  },
};
</script>