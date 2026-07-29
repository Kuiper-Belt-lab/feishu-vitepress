<script setup lang="ts">
import { onMounted, onUnmounted, watch, nextTick } from "vue";
import DefaultTheme from "vitepress/theme";
import { useRouter } from "vitepress";
import mediumZoom from "medium-zoom";
import CategoryNav from "./CategoryNav.vue";
import ArticleMeta from "./ArticleMeta.vue";
import ArticleList from "./ArticleList.vue";
import ArticleComment from "./ArticleComment.vue";
import ArticleCopyright from "./ArticleCopyright.vue";
import BusuanziStats from "./BusuanziStats.vue";
const { Layout } = DefaultTheme;

const router = useRouter();
const initImagesZoom = () => {
  mediumZoom(".main img", {
    background: "var(--vp-c-bg)",
  });
};

onMounted(() => {
  nextTick(() => {
    if (
      localStorage.theme === "dark" ||
      (!("theme" in localStorage) &&
        window.matchMedia("(prefers-color-scheme: dark)").matches)
    ) {
      document.documentElement.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
    }

    // Whenever the user explicitly chooses light mode
    localStorage.theme = "light";

    // Whenever the user explicitly chooses dark mode
    localStorage.theme = "dark";
    // Whenever the user explicitly chooses to respect the OS preference
    localStorage.removeItem("theme");
    initImagesZoom();
  });
});
watch(router.route, () => {
  // 清除上一次的监听
  nextTick(() => {
    initImagesZoom();
  });
});
onUnmounted(() => {});
</script>

<template>
  <Layout>
    <template #nav-bar-title-after>
      <span class="brand-title">
        <span class="brand-title__main">Kuiper-Belt</span>
        <span class="brand-title__suffix">AI Lab</span>
      </span>
    </template>
    <!-- 文章顶部模块 -->
    <template #doc-before>
      <ArticleMeta />
    </template>
    <template #doc-bottom> </template>
    <template #doc-footer-before>
      <ClientOnly>
        <ArticleCopyright />
        <BusuanziStats />
      </ClientOnly>
    </template>
    <!-- 文章尾部 -->
    <template #doc-after>
      <!-- 评论模块 -->
      <ArticleComment />
    </template>
    <template #aside-outline-before> </template>
    <template #home-hero-before>
      <ClientOnly>
        <CategoryNav />
      </ClientOnly>
    </template>
    <!-- 主页模块 -->
    <template #home-hero-after>
      <!-- 首页文章列表模块 -->
      <ArticleList />
      <ClientOnly>
        <div class="flex justify-center">
          <BusuanziStats />
        </div>
      </ClientOnly>
    </template>
  </Layout>
</template>

<style scoped>
.brand-title {
  display: inline-flex;
  align-items: baseline;
  gap: 0.35rem;
  line-height: 1;
  white-space: nowrap;
}

.brand-title__main {
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: -0.01em;
}

.brand-title__suffix {
  color: var(--vp-c-text-2);
  font-size: 0.78rem;
  font-weight: 400;
  letter-spacing: 0.04em;
}
</style>
