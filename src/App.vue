<template>
  <v-app>
    <v-main>
<el-header>
  <el-menu :default-active="activeIndex" mode="horizontal"
  router>

  <el-menu-item index='home'  :route="{ name: 'home' }">Todo</el-menu-item>
  <el-menu-item index='about' :route="{ name: 'about' }">about</el-menu-item>

  </el-menu>
</el-header>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
}
</script>
