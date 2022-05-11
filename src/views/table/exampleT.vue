<!-- 命令集 -->
<template>
  <div class="app-container">
    <el-input v-model="input" style="width:15vw" placeholder="请输入内容"></el-input>
    <el-button type="primary" plain>搜索</el-button>
    <el-button type="primary" plain @click="add()">新增命令集</el-button>
    <el-table v-loading="listLoading" :data="list" element-loading-text="Loading" border fit highlight-current-row>
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.$index }}
        </template>
      </el-table-column>
      <el-table-column label="文本">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="扩展" width="110" align="center">
        <template slot-scope="scope">
          {{ scope.row.pageviews }}
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="启用状态" width="110" align="center">
        <template slot-scope="scope">
          <el-switch v-model="scope.row.status">
            <!-- active-text="按月付费" inactive-text="按年付费"> -->
          </el-switch>
          <!-- <el-tag :type="scope.row.status | statusFilter">{{ scope.row.status }}</el-tag> -->
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="操作" width="200">
        <template slot-scope="scope">
          <el-button type="danger" icon="el-icon-delete" @click="dele(scope.row.id)" circle></el-button>
          <el-button type="primary" icon="el-icon-edit" circle></el-button>
          <!-- <i class="el-icon-time" /> -->
          <!-- <span>{{ scope.row.display_time }}</span> -->
        </template>
      </el-table-column>
    </el-table>

    <!-- 新增 -->
    <el-dialog title="命令集" :visible.sync="addVisible">
      <el-form :model="form" :label-position="'right'" label-width="80px">
        <el-form-item label="文本" >
          <el-input v-model="form.name" style="width: 20vw;" autocomplete="no"></el-input>
        </el-form-item>
        <el-form-item label="扩展" >
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="启用状态" >
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitFun()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { getList } from '@/api/table'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      form: {},
      input: '',
      listLoading: true,
      addVisible: false,
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      getList().then(response => {
        this.list = response.data.items
        this.listLoading = false
      })
    },
    add() {
      this.addVisible = true
    },
    dele() {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
        center: true
      }).then(() => {
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    submitFun() {
      this.addVisible = false
    }
  }

}
</script>
