<template>
  <!--增删改查-->
  <div>
    <!--按钮、表格-->
    <el-card class="box-card">
      <el-button type="success" size="small" @click="add">增加</el-button>
      <el-table ref="filterTable" :data="tableData" stripe>
        <el-table-column prop="title" label="项目名" align="center" />
        <el-table-column prop="date" label="日期" align="center" sortable width="100" column-key="project" :filters="[{text:'2020-01-02', value:'2020-01-02'},{text:'2020-05-30', value:'2020-05-30'},{text:'2022-09-30', value:'2022-09-30'}]" :filter-method="filterHandler" />
        <el-table-column prop="name" label="负责人" align="center" />
        <el-table-column prop="address" label="客户地址" align="center" />
        <el-table-column prop="postcode" label="邮编" align="center" />
        <el-table-column prop="tag" label="标签" align="center" sortable width="100" conlumn-key="tag" :filters="[{text:'项目进行中', value:'项目进行中'},{text:'项目即将到期', value:'项目即将到期'},{text:'项目已到期', value:'项目已到期'}]" :filter-method="filterHandler" />
        <!-- <template slot-scope="tagscope">
            <el-tag :type="scope.row.tag === '项目进行中' ? '项目即将到期' : '项目已到期'" disable-transitions>{{tagscope.row.tag}}</el-tag>
        </template>
        </el-table-column> -->
        <el-table-column prop="comment" label="备注" align="center" />
        <el-table-column label="操作" fixed="right" align="center">
          <template slot-scope="scope">
            <el-button type="primary" size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
            <el-button type="danger" size="small" @click="remove(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
    <!--弹窗-->
    <el-dialog :title="dialogTitle" width="50%" :visible.sync="iconFormVisible">
      <el-form :inline="true" :model="userInfo" class="demo-form-inline">
        <el-form-item label="项目名">
          <el-input v-model="userInfo.title" placeholder="项目名" />
        </el-form-item>
        <el-form-item label="日期">
          <el-input v-model="userInfo.date" placeholder="eg.xxxx-xxxx" />
        </el-form-item>
        <el-form-item label="负责人">
          <el-input v-model="userInfo.name" placeholder="负责人" />
        </el-form-item>
        <el-form-item label="客户地址">
          <el-input v-model="userInfo.address" placeholder="客户地址" />
        </el-form-item>
        <el-form-item label="邮编">
          <el-input v-model="userInfo.postcode" placeholder="eg.110-1111" />
        </el-form-item>
        <el-form-item label="备注">
          <el-input v-model="userInfo.comment" placeholder="备注" />
        </el-form-item>
        <el-form-item label="标签">
          <el-input v-model="userInfo.tag" placeholder="标签" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="iconFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitUser()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'Dragtable',
  data() {
    return {
      iconFormVisible: false,
      userInfo: {},
      dialogTitle: '增加',
      rowIndex: null,
      tableData: [{
        title: '银行项目',
        date: '2020-01-02',
        name: '于XX',
        address: '东京都江东区',
        postcode: '110-0015',
        comment: ''
      }, {
        title: '软银项目',
        date: '2020-05-30',
        name: '杨XX',
        address: '东京都千代田区',
        postcode: '123-1111',
        comment: ''
      }, {
        title: 'HTKK项目',
        date: '2022-09-30',
        name: '范XX',
        address: '东京都丰岛区',
        postcode: '123-1122',
        comment: ''
      }]
    }
  },
  created() {},
  methods: {
    // 增加
    add() {
      this.dialogTitle = '增加'
      this.userInfo = {}
      this.iconFormVisible = true
    },
    // 编辑
    handleEdit(index, row) {
      this.dialogTitle = '编辑'
      this.userInfo = row
      this.iconFormVisible = true
      this.rowIndex = index
    },
    // 弹窗确定
    submitUser() {
      if (this.dialogTitle === '编辑') {
        this.tableData.splice(this.rowIndex, 1, this.userInfo)
        this.iconFormVisible = false
        return
      }
      this.tableData.splice(0, 0, this.userInfo)
      this.iconFormVisible = false
    },
    // 删除
    remove(index, row) {
      this.$confirm(`确定删除${row.title}吗?`, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'error'
      }).then(() => {
        this.tableData.splice(index, 1)
      })
    },
    filterTag(value, row) {
      return row.tag === value
    },
    filterHandler(value, row, column) {
      const property = column['property']
      return row[property] === value
    }
  }
}
</script>
