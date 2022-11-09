<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right" style="font-size: 16px;">
      <el-breadcrumb-item :to="{ path: '/' }">后台管理</el-breadcrumb-item>
      <el-breadcrumb-item>管理员列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-divider></el-divider>

    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="id" label="ID" width="40" align="center"></el-table-column>
      <el-table-column prop="username" label="用户名" width="120" align="center"></el-table-column>
      <el-table-column prop="nickname" label="昵称" width="100" align="center"></el-table-column>
      <el-table-column prop="phone" label="手机号码" width="120" align="center"></el-table-column>
      <el-table-column prop="email" label="电子邮箱" width="180" align="center"></el-table-column>
      <el-table-column prop="description" label="简介" header-align="center"
                       :show-overflow-tooltip="true"></el-table-column>
      <el-table-column label="是否启用" width="80" align="center">
        <template slot-scope="scope">
          <el-switch
              @change="changeEnable(scope.row)"
              v-model="scope.row.enable"
              :disabled="scope.row.id == 1"
              :active-value="1"
              :inactive-value="0"
              active-color="#13ce66"
              inactive-color="#AAAAAA">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="120" align="center">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" circle size="mini"
                     :disabled="scope.row.id == 1"
                     @click="handleEdit(scope.row)"></el-button>
          <el-button type="danger" icon="el-icon-delete" circle size="mini"
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
    changeEnable(admin) {
      let enableText = ['禁用', '启用'];
      let url = 'http://localhost:9081/admins/' + admin.id;
      if (admin.enable == 1) {
        console.log('将启用管理员');
        url += '/enable';
      } else {
        console.log('将禁用管理员');
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
    handleEdit(admin) {
      let message = '您正在尝试编辑【' + admin.id + '-' + admin.username + '】的管理员详情，抱歉，该功能尚未实现……';
      this.$alert(message, '提示', {
        confirmButtonText: '确定'
      });
    },
    handleDelete(admin) {
      let url = 'http://localhost:9081/admins/' + admin.id + '/delete';
      console.log('url = ' + url);
      this.axios.post(url).then((response) => {
        let responseBody = response.data;
        if (responseBody.state != 20000) {
          this.$message.error(responseBody.message);
        }
        if (responseBody.state == 20000 || responseBody.state == 40400) {
          this.loadAdminList();
        }
      });
    },
    openDeleteConfirm(admin) {
      let message = '此操作将永久删除【' + admin.username + '】管理员，是否继续？';
      this.$confirm(message, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.handleDelete(admin);
      }).catch(() => {
      });
    },
    loadAdminList() {
      console.log('loadAdminList');
      console.log('在localStorage中的JWT数据：' + localStorage.getItem('jwt'));
      let url = 'http://localhost:9081/admins';
      console.log('url = ' + url);
      this.axios
          .create({headers: {'Authorization': 'Bearer ' + localStorage.getItem('jwt')}})
          .get(url).then((response) => {
        let responseBody = response.data;
        this.tableData = responseBody.data;
      });
    }
  },
  mounted() {
    console.log('mounted');
    this.loadAdminList();
  }
}
</script>