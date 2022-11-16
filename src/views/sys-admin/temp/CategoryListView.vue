<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right" style="font-size: 16px;">
      <el-breadcrumb-item :to="{ path: '/sys-admin' }">
        <i class="el-icon-s-promotion"></i> 后台管理
      </el-breadcrumb-item>
      <el-breadcrumb-item>类别列表</el-breadcrumb-item>
      <el-breadcrumb-item v-for="item in history"><span v-text="item.name"></span></el-breadcrumb-item>
    </el-breadcrumb>

    <el-divider></el-divider>

    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="id" label="ID" width="60" align="center"></el-table-column>
      <el-table-column prop="name" label="名称" width="180" align="center"></el-table-column>
      <el-table-column prop="description" label="简介" align="center"></el-table-column>
      <el-table-column prop="sort" label="排序序号" width="80" align="center"></el-table-column>
      <el-table-column label="是否启用" width="80" align="center">
        <template slot-scope="scope">
          <el-switch
              @change="handleChangeEnable(scope.row)"
              v-model="scope.row.enable"
              :active-value="1"
              :inactive-value="0"
              active-color="#13ce66"
              inactive-color="#999">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="显示在导航栏" width="120" align="center">
        <template slot-scope="scope">
          <el-switch
              @change="handleChangeDisplay(scope.row)"
              v-model="scope.row.isDisplay"
              :active-value="1"
              :inactive-value="0"
              active-color="#13ce66"
              inactive-color="#999">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="查看子级" width="100" align="center">
        <template slot-scope="scope">
          <el-button size="mini" :disabled="scope.row.isParent == 0"
                     @click="showSubCategories(scope.row)">查看子级
          </el-button>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="100" align="center">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" size="mini" circle
                     @click="handleEdit(scope.row)"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini" circle
                     :disabled="scope.row.isParent == 1"
                     @click="openDeleteConfirm(scope.row)"></el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-button style="margin-top: 10px; float: right;"
               v-if="currentDepth != 1" @click="goBack()">返回
    </el-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      history: [],
      currentDepth: 1,
      currentParentId: 0,
      enableText: ['禁用', '启用'],
      displayText: ['不显示在导航栏', '显示在导航栏'],
      tableData: []
    }
  },
  methods: {
    goBack() {
      let parentCategory = this.history[--this.currentDepth - 1];
      this.currentParentId = parentCategory.parentId;
      this.history.pop();
      this.loadCategoryList();
    },
    showSubCategories(category) {
      this.history[this.currentDepth - 1] = category;
      this.currentDepth++;
      this.currentParentId = category.id;
      this.loadCategoryList();
    },
    handleChangeEnable(category) {
      let url = 'http://localhost:9080/categories/' + category.id;
      if (category.enable == 1) {
        url += '/enable';
      } else {
        url += '/disable';
      }
      console.log('url = ' + url);
      this.axios
          .create({'headers': {'Authorization': localStorage.getItem('jwt')}})
          .post(url).then((response) => {
        let responseBody = response.data;
        if (responseBody.state == 20000) {
          this.$message({
            message: '操作成功，已经将【' + category.name + '】的状态改为【' + this.enableText[category.enable] + '】！',
            type: 'success'
          });
        } else {
          this.$message.error(responseBody.message);
        }
        this.loadCategoryList();
      });
    },
    handleChangeDisplay(category) {
      let url = 'http://localhost:9080/categories/' + category.id;
      if (category.isDisplay == 1) {
        url += '/display';
      } else {
        url += '/hidden';
      }
      console.log('url = ' + url);
      this.axios
          .create({'headers': {'Authorization': localStorage.getItem('jwt')}})
          .post(url).then((response) => {
        let responseBody = response.data;
        if (responseBody.state == 20000) {
          this.$message({
            message: '操作成功，已经将【' + category.name + '】的状态改为【' + this.displayText[category.isDisplay] + '】！',
            type: 'success'
          });
        } else {
          this.$message.error(responseBody.message);
        }
        this.loadCategoryList();
      });
    },
    handleEdit(category) {
      let title = '提示';
      let message = '您正在尝试编辑【' + category.id + '-' + category.name + '】的类别详情，抱歉，此功能尚未实现……';
      this.$alert(message, title, {
        confirmButtonText: '确定'
      });
    },
    openDeleteConfirm(category) {
      let title = '提示';
      let message = '此操作将永久删除【' + category.name + '】类别，是否继续？';
      this.$confirm(message, title, {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.handleDelete(category);
      }).catch(() => {
      });
    },
    handleDelete(category) {
      let url = 'http://localhost:9080/categories/' + category.id + '/delete';
      console.log('url = ' + url);
      this.axios
          .create({'headers': {'Authorization': localStorage.getItem('jwt')}})
          .post(url).then((response) => {
        let responseBody = response.data;
        if (responseBody.state != 20000) {
          this.$message.error(responseBody.message);
        }
        this.loadCategoryList();
      });
    },
    loadCategoryList() {
      let url = 'http://localhost:9080/categories/list-by-parent?parentId=' + this.currentParentId;
      console.log('url = ' + url);
      this.axios
          .create({'headers': {'Authorization': localStorage.getItem('jwt')}})
          .get(url).then((response) => {
        let responseBody = response.data;
        if (responseBody.state == 20000) {
          this.tableData = responseBody.data;
        } else {
          this.$message.error(responseBody.message);
        }
      });
    }
  },
  mounted() {
    this.loadCategoryList();
  }
}
</script>

<style scoped>

</style>