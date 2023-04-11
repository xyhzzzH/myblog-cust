<template>
  <div>
    <div class="top">
      <div class="top-container">
        <div class="logo"><a href="">Yu Blog</a></div>
        <div class="nav">
          <span class="iconfont icon-blog"></span>
          <router-link to="/" :class="[currentPath == '/' ? 'active' : '']"
            >博客</router-link
          >
          <span class="iconfont icon-fenlei"></span>
          <router-link
            to="/category"
            :class="[currentPath == '/category' ? 'active' : '']"
            >分类专栏</router-link
          >
          <span class="iconfont icon-zhuanti"></span>
          <router-link
            to="/special"
            :class="[currentPath == '/special' ? 'active' : '']"
            >专题</router-link
          >
          <span class="iconfont icon-chengyuan"></span>
          <router-link
            to="/user"
            :class="[currentPath == '/user' ? 'active' : '']"
            >成员</router-link
          >
          <span class="iconfont icon-liuyan"></span>
          <router-link
            to="/comment"
            :class="[currentPath == '/comment' ? 'active' : '']"
            >留言板</router-link
          >
        </div>
      </div>
      <div class="center-container">
        <div>Yu Blog</div>
        <div>想多了全是问题，做多了全是答案。</div>
      </div>
      <span class="iconfont icon-xiangxia"></span>
    </div>
    <div class="body-container">
      <el-config-provider :message="config" size="default">
        <!--  <router-view v-slot="{ Component }">
          <transition name="fade"
                      mode="out-in">
            <keep-alive>
              <component :is="Component"
                         :key="Component" />
            </keep-alive>
          </transition>
        </router-view> -->
        <router-view />
      </el-config-provider>
    </div>
    <div class="footer" v-if="sysSetting.showIcp">
      <div>
        <span
          >©2021-{{ new Date().getFullYear() }} {{ sysSetting.icpDomain }} All
          rights reserved.
        </span>
        <a target="_blank" href="https://beian.miit.gov.cn/">{{
          sysSetting.icpNo
        }}</a>
      </div>
      <div>
        <img src="@/assets/beian.png" />
        <a
          target="_blank"
          :href="
            'http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=' +
            sysSetting.policeNo
          "
        >
          鄂公网安备 {{ sysSetting.policeNo }}号
        </a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch, getCurrentInstance } from "vue";
import { useRouter } from "vue-router";
const router = useRouter();
const { proxy } = getCurrentInstance();
const config = reactive({
  max: 1,
});

const currentPath = ref("/");

const initTitlt = () => {
  document.location.title = "web";
};
initTitlt();

watch(
  () => router.currentRoute.value.meta.parentPath,
  (newVal, oldValue) => {
    currentPath.value = newVal;
  },
  { immediate: true, deep: true }
);

const sysSetting = ref({});
const loadSysInfo = async () => {
  let result = await proxy.Request({
    url: "/view/getSysInfo",
  });
  if (!result) {
    return;
  }
  sysSetting.value = result.data;
};
loadSysInfo();
</script>

<style lang="scss" scoped>
</style>
