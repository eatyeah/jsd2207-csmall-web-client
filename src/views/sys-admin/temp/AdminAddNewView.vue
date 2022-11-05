<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right" style="font-size: 16px;">
      <el-breadcrumb-item :to="{ path: '/sys-admin' }">
        <i class="el-icon-s-promotion"></i> 后台管理
      </el-breadcrumb-item>
      <el-breadcrumb-item>添加管理员</el-breadcrumb-item>
    </el-breadcrumb>

    <el-divider></el-divider>

    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="130px" class="demo-ruleForm">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="ruleForm.username"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="ruleForm.password"></el-input>
      </el-form-item>
      <el-form-item label="昵称" prop="nickname">
        <el-input v-model="ruleForm.nickname"></el-input>
      </el-form-item>
      <el-form-item label="简介" prop="description">
        <el-input v-model="ruleForm.description"></el-input>
      </el-form-item>
      <el-form-item label="手机号码" prop="phone">
        <el-input v-model="ruleForm.phone"></el-input>
      </el-form-item>
      <el-form-item label="电子邮箱" prop="email">
        <el-input v-model="ruleForm.email"></el-input>
      </el-form-item>
      <el-form-item label="头像" prop="avatar">
        <el-input v-model="ruleForm.avatar"></el-input>
      </el-form-item>
      <el-form-item label="是否启用" prop="enable">
        <el-switch
            v-model="ruleForm.enable"
            :active-value="1"
            :inactive-value="0"
            active-color="#13ce66"
            inactive-color="#ccc">
        </el-switch>
      </el-form-item>
      <el-form-item label="角色" prop="roleIds">
        <el-select v-model="ruleForm.roleIds" multiple placeholder="请选择">
          <el-option
              v-for="item in roleList"
              :key="item.id"
              :label="item.name"
              :value="item.id">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">添加</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>

<script>
export default {
  data() {
    return {
      roleList: [],
      ruleForm: {
        username: '',
        password: '',
        nickname: '',
        description: '',
        phone: '',
        email: '',
        avatar: '',
        enable: '',
        roleIds: ''
      },
      rules: {
        username: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
          {min: 4, max: 15, message: '长度在 2 到 15 个字符', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 4, max: 15, message: '长度在 2 到 15 个字符', trigger: 'blur'}
        ],
        nickname: [
          {required: true, message: '请输入昵称', trigger: 'blur'},
          {min: 2, max: 15, message: '长度在 2 到 15 个字符', trigger: 'blur'}
        ],
        phone: [
          {required: true, message: '请输入手机号码', trigger: 'blur'},
          {min: 8, max: 15, message: '长度在 8 到 15 个字符', trigger: 'blur'}
        ],
        email: [
          {required: true, message: '请输入电子邮箱', trigger: 'blur'},
          {min: 4, max: 35, message: '长度在 5 到 35 个字符', trigger: 'blur'}
        ],
        enable: [
          {required: true, message: '', trigger: 'change'}
        ],
        roleIds: [
          {type: 'array', required: true, message: '请选择角色', trigger: 'blur'}
        ]
      }
    };
  },
  methods: {
    // 获取角色列表
    loadRoleList() {
      console.log('loadRoleList');
      let url = 'http://localhost:9081/roles';
      console.log('url = ' + url);
      this.axios.get(url).then((response) => {
        let responseBody = response.data;
        this.roleList = responseBody.data;
      });
    },
    // 提交表单
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          let url = 'http://localhost:9081/admins/add-new';
          console.log('url = ' + url);
          let formData = this.qs.stringify(this.ruleForm,{arrayFormat: 'repeat'});
          console.log('formData：' + formData);
          this.axios.post(url, formData).then((response)=> {
            let responseBody = response.data;
            if (responseBody.state == 20000) {
              this.$message({
                message: '添加管理员成功！',
                type: 'success'
              });
              this.resetForm(formName);
            } else {
              this.$message.error(responseBody.message);
            }
          });
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  },
  mounted() {
    this.loadRoleList();
  }
}
</script>

<style scoped>

</style>
