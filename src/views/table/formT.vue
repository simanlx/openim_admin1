<!-- 科室 -->
<template>
  <div class="app-container">
    <el-input v-model="input" style="width:15vw" placeholder="请输入内容"></el-input>
    <el-button type="primary" plain>搜索</el-button>
    <el-button type="primary" plain>新增科室</el-button>
    <el-table v-loading="listLoading" :data="list" element-loading-text="Loading" border fit highlight-current-row>
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.$index }}
        </template>
      </el-table-column>
      <!-- <el-table-column label="姓名">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column> -->
      <el-table-column label="科室" width="110" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.author }}</span>
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
          <el-button type="danger" icon="el-icon-delete" circle></el-button>
          <el-button type="primary" icon="el-icon-edit" circle></el-button>
          <!-- <i class="el-icon-time" /> -->
          <!-- <span>{{ scope.row.display_time }}</span> -->
        </template>
      </el-table-column>
    </el-table>
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
      listLoading: true
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
    }
  }
}
</script>
