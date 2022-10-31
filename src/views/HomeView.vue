<template>
  <div class="home">

    <!-- 最外层容器 -->
    <el-container>

      <!-- 上半部分：顶栏 -->
      <el-header class="layout-header">
        <h1>酷鲨商城运营管理平台</h1>
      </el-header>

      <!-- 下半部分：容器 -->
      <el-container class="layout-body">

        <!-- 下半部分的左侧边栏 -->
        <el-aside class="layout-aside">
          <el-menu
              default-active="2"
              class="el-menu-vertical-demo"
              background-color="#545c64"
              text-color="#fff"
              active-text-color="#ffd04b">
            <el-menu-item index="0">
              <i class="el-icon-s-home"></i>
              <span slot="title">首页</span>
            </el-menu-item>
            <el-submenu index="2">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>临时页面</span>
              </template>
              <el-menu-item index="1">
                <i class="el-icon-user-solid"></i>
                <span slot="title">添加管理员</span>
              </el-menu-item>
              <el-menu-item index="2">
                <i class="el-icon-user"></i>
                <span slot="title">管理员列表</span>
              </el-menu-item>
              <el-menu-item index="3">
                <i class="el-icon-circle-plus"></i>
                <span slot="title">添加品牌</span>
              </el-menu-item>
              <el-menu-item index="4">
                <i class="el-icon-setting"></i>
                <span slot="title">品牌列表</span>
              </el-menu-item>
              <el-menu-item index="5">
                <i class="el-icon-setting"></i>
                <span slot="title">添加类别</span>
              </el-menu-item>
              <el-menu-item index="6">
                <i class="el-icon-setting"></i>
                <span slot="title">类别列表</span>
              </el-menu-item>
              <el-menu-item index="7">
                <i class="el-icon-setting"></i>
                <span slot="title">添加相册</span>
              </el-menu-item>
              <el-menu-item index="8">
                <i class="el-icon-setting"></i>
                <span slot="title">相册列表</span>
              </el-menu-item>
              <el-menu-item index="9">
                <i class="el-icon-setting"></i>
                <span slot="title">添加属性</span>
              </el-menu-item>
              <el-menu-item index="10">
                <i class="el-icon-setting"></i>
                <span slot="title">属性列表</span>
              </el-menu-item>
              <el-menu-item index="11">
                <i class="el-icon-setting"></i>
                <span slot="title">添加属性模板</span>
              </el-menu-item>
              <el-menu-item index="12">
                <i class="el-icon-setting"></i>
                <span slot="title">属性模板列表</span>
              </el-menu-item>
            </el-submenu>
            <el-submenu index="3">
              <template slot="title">
                <i class="el-icon-goods"></i>
                <span>商品管理</span>
              </template>
              <el-menu-item index="13">
                <i class="el-icon-setting"></i>
                <span slot="title">SPU台帐</span>
              </el-menu-item>
              <el-menu-item index="14">
                <i class="el-icon-setting"></i>
                <span slot="title">新增SPU</span>
              </el-menu-item>
              <el-menu-item index="15">
                <i class="el-icon-setting"></i>
                <span slot="title">相册管理</span>
              </el-menu-item>
              <el-menu-item index="16">
                <i class="el-icon-setting"></i>
                <span slot="title">商品类别</span>
              </el-menu-item>
              <el-menu-item index="17">
                <i class="el-icon-setting"></i>
                <span slot="title">属性模板</span>
              </el-menu-item>
              <el-menu-item index="18">
                <i class="el-icon-setting"></i>
                <span slot="title">商品管理</span>
              </el-menu-item>
            </el-submenu>
            <el-submenu index="4">
              <template slot="title">
                <i class="el-icon-s-order"></i>
                <span>订单管理</span>
              </template>
            </el-submenu>
            <el-submenu index="5">
              <template slot="title">
                <i class="el-icon-s-marketing"></i>
                <span>营销管理</span>
              </template>
            </el-submenu>
            <el-submenu index="6">
              <template slot="title">
                <i class="el-icon-s-shop"></i>
                <span>商家管理</span>
              </template>
            </el-submenu>
            <el-submenu index="7">
              <template slot="title">
                <i class="el-icon-s-tools"></i>
                <span>权限管理</span>
              </template>
            </el-submenu>
          </el-menu>
        </el-aside>

        <!-- 下半部分的右侧主体部分 -->
        <el-main class="layout-main">

        </el-main>

      </el-container>

    </el-container>

  </div>

</template>

<script>

</script>

<style>
.layout-header {
  /*background: #1684b0;*/
  background: #00aaff;
}

.layout-header h1 {
  color: #fff;
  text-align: left;
  line-height: 60px;
  font-size: 24px;
}

.layout-body {
  position: absolute;
  top: 60px;
  bottom: 0;
  left: 0;
  right: 0;
}

.layout-aside {
  /*background: #222C32;*/
  background: #545C64;
}

.el-menu-item.is-active {
  /*important，当你的样式被覆盖时，可以使用important来提高优先级*/
  background: #2D3C4D !important;
}

.layout-aside i{
  color: #fff !important;
}

.el-menu-vertical-demo {

}

.layout-main {

}
</style>