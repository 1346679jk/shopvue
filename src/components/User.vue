<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 用户对话框 -->
    <el-dialog title="添加用户" :visible.sync="addUserDialog" width="50%">
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addUserDialog = false">取 消</el-button>
        <el-button type="primary" @click="addUserDialog = false">确 定</el-button>
      </span>
    </el-dialog>

    <!-- 卡片区 -->
    <el-card class="box-card">
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input
            v-model="querycdt.query"
            placeholder="请输入搜索内容"
            clearable
            @clear="search()"
            @keyup.enter.native="search()"
            class="input-with-select"
          >
            <el-button slot="append" icon="el-icon-search" @click="search()"></el-button>
          </el-input>
        </el-col>
        <el-col :span="8">
          <el-button type="primary" @click="addUserDialog=true">添加用户</el-button>
        </el-col>
      </el-row>

      <!-- 数据列表区 -->
      <el-table :data="userList" border stripe style="width: 100%">
        <el-table-column label="序号" width="60" type="index"></el-table-column>
        <el-table-column prop="username" label="用户名" width="190"></el-table-column>
        <el-table-column prop="mobile" label="手机号码" width="140"></el-table-column>
        <el-table-column prop="role_name" label="角色" width="140"></el-table-column>
        <el-table-column prop="email" label="邮箱" width="160"></el-table-column>
        <el-table-column prop="mg_state" label="状态" width="60">
          <el-switch v-model="info.row.mg_state" slot-scope="info"></el-switch>
        </el-table-column>
        <el-table-column label="操作" width="300">
          <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
          <el-tooltip class="item" effect="dark" content="分配角色" placement="top">
            <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
          </el-tooltip>
        </el-table-column>
      </el-table>

      <!-- 分页 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="querycdt.pagenum"
        :page-sizes="[3, 5, 10, 20]"
        :page-size="3"
        layout="total, sizes, prev, pager, next, jumper"
        :total="tot"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  created () {
    this.getUserList()
  },
  data () {
    return {
      addUserDialog: false,
      userList: [],
      tot: 0,
      querycdt: {
        query: '',
        pagenum: 1,
        pagesize: 3
      }
    }
  },
  methods: {
    async getUserList () {
      const { data: dt } = await this.$http.get('/users', {
        params: this.querycdt
      })
      if (dt.meta.status !== 200) {
        return this.$message.error(dt.meta.msg)
      }
      this.tot = dt.data.total
      this.userList = dt.data.users
    },
    search () {
      this.getUserList()
    },
    handleCurrentChange (val) {
      this.querycdt.pagenum = val
      this.getUserList()
    },
    handleSizeChange (val) {
      this.querycdt.pagesize = val
      this.getUserList()
    }
  }
}
</script>

<style lang="less" scoped>
</style>
