<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right" style="font-size: 16px;">
      <el-breadcrumb-item :to="{ path: '/sys-admin' }">
        <i class="el-icon-s-promotion"></i> 后台管理
      </el-breadcrumb-item>
      <el-breadcrumb-item>管理员列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-divider></el-divider>

    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="id" label="ID" align="center" width="40"></el-table-column>
      <el-table-column prop="username" label="用户名" align="center" width="120"></el-table-column>
      <el-table-column prop="nickname" label="昵称" align="center" width="100"></el-table-column>
      <el-table-column prop="phone" label="手机号码" align="center" width="110"></el-table-column>
      <el-table-column prop="email" label="电子邮箱" header-align="center" width="180"></el-table-column>
      <el-table-column prop="description" label="简介" header-align="center"></el-table-column>
      <el-table-column label="是否启用" align="center" width="80">
        <template slot-scope="scope">
          <el-switch
              @change="changeEnable(scope.row)"
              v-model="scope.row.enable"
              :active-value="1"
              :inactive-value="0"
              active-color="#13ce66"
              inactive-color="#ccc">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="100">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" size="mini" circle
                     :disabled="scope.row.id == 1"
                     @click="handleEdit(scope.row)"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini" circle
                     :disabled="scope.row.id == 1"
                     @click="openDeleteConfirm(scope.row)"></el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: []
    }
  },
  methods: {
    // 是否启用管理员
    changeEnable(admin) {
      let enableText = ['禁用', '启用'];
      let url = 'http://localhost:9081/admins/' + admin.id;
      if (admin.enable == 1) {
        url += '/enable';
      } else {
        url += '/disable';
      }
      console.log('url = ' + url);
      this.axios.post(url).then((response) => {
        let responseBody = response.data;
        if (responseBody.state == 20000) {
          let message = '将管理员【' + admin.username + '】的启用状态改为【' + enableText[admin.enable] + '】成功！';
          this.$message({
            message: message,
            type: 'success'
          });
        } else {
          this.$message.error(responseBody.message);
        }
        if (responseBody.state == 40400) {
          this.loadAdminList();
        }
      });
    },
    // 编辑管理员
    handleEdit(admin) {
      let title = '提示';
      let message = '您正在尝试编辑【' + admin.id + '-' + admin.username + '】的管理员详情，抱歉，此功能尚未实现……';
      this.$alert(message, title, {
        confirmButtonText: '确定'
      });
    },
    // 点击删除按钮
    openDeleteConfirm(admin) {
      let title = '提示';
      let message = '此操作将永久删除【' + admin.username + '】管理员，是否继续？';
      this.$confirm(message, title, {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.handleDelete(admin);
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
      });
    },
    // 删除管理员
    handleDelete(admin) {
      console.log('handleDelete ... id=' + admin.id);
      let url = 'http://localhost:9081/admins/' + admin.id + '/delete';
      console.log('url = ' + url);
      this.axios.post(url).then((response) => {
        let responseBody = response.data;
        console.log(responseBody);
        if (responseBody.state != 20000) {
          this.$message.error(responseBody.message);
        }
        if (responseBody.state == 20000 || responseBody.state == 40400) {
          this.loadAdminList();
        }
      });
    },
    // 加载管理员列表
    loadAdminList() {
      console.log('loadAdminList ...');
      console.log('在localStorage中的JWT数据' + localStorage.getItem('jwt'));
      let url = 'http://localhost:9081/admins';
      console.log('url = ' + url);
      this.axios.get(url).then((response) => {
        let responseBody = response.data;
        console.log(responseBody);
        this.tableData = responseBody.data;
      });
    }
  },
  mounted() {
    this.loadAdminList();
  }
}
</script>
