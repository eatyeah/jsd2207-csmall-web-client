<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">后台管理</el-breadcrumb-item>
      <el-breadcrumb-item>添加相册</el-breadcrumb-item>
      <el-divider></el-divider>
    </el-breadcrumb>

    <el-empty description="暂无数据"></el-empty>
  </div>
</template>
