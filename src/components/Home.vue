<template>
  <el-container class="home_container">
    <el-header>
      <div>
        <span>Shop Management System</span>
      </div>
      <el-button type="info" @click="logout">退出系統</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <el-menu background-color="#333744" text-color="#fff" active-text-color="#409bff"
                 unique-opened :collapse="isCollapse" :collapse-transition="false" router>
          <div class="toggle-button" @click="toggleCollapse">|||</div>
          <!-- 一级菜单 -->
          <el-submenu :index="obj.id + ''" v-for="(obj) in menuList" :key="obj.id">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{obj.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <div v-if="obj.children">
              <el-menu-item :index="'/' + item.path" v-for="(item) in obj.children" :key="item.id">
                <template slot="title">
                  <i class="el-icon-menu"></i>
                  <span>{{item.authName}}</span>
                </template>
              </el-menu-item>
            </div>
          </el-submenu>

        </el-menu>
      </el-aside>

      <el-main>
        <!-- 子路由占位符 -->
        <router-view/>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
    export default {
        name: "Home",
        data() {
            return {
                menuList: [],
                isCollapse: false
            }
        },
        created() {
            // 页面加载的时候请求侧边栏菜单数据
            this.getMenuList();
        },
        methods: {
            logout() {
                window.sessionStorage.clear();
                this.$router.push('/login');
            },
            async getMenuList() {
                const {data: res} = await this.$http.get('menus');
                if(res.meta.status !== 200) return this.$message.error(res.meta.msg);
                this.menuList = res.data;
            },
            toggleCollapse() {
              this.isCollapse = !this.isCollapse;
            }
        }
    }
</script>

<style lang="less" scoped>
.home_container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 5px;
  color: #fff;
  font-size: 20px;
  align-items: center;
}
.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #eaedf1;
}
.toggle-button {
  background-color: slategray;
  font-size: 12px;
  color: #fff;
  line-height: 24px;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
