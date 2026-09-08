<script setup lang="ts">
import { computed } from 'vue'
import DefaultTheme from 'vitepress/theme'
import { useData } from 'vitepress'
import MoocHome from './components/MoocHome.vue'
import ArticlesExplore from './components/ArticlesExplore.vue'
import ArticleHero from './components/ArticleHero.vue'
import RelatedArticles from './components/RelatedArticles.vue'
import MoocHeader from './components/MoocHeader.vue'

const { frontmatter, page } = useData()

/** VitePress 只认 home / doc / page；layout: article 会被当成空的 <article> 标签 */
const isArticle = computed(() => page.value.relativePath.startsWith('articles/'))
</script>

<template>
  <MoocHome v-if="frontmatter.layout === 'mooc-home'" />
  <ArticlesExplore v-else-if="frontmatter.layout === 'articles-explore'" />
  <div v-else class="site-shell" :class="{ 'is-article': isArticle }">
    <MoocHeader />
    <DefaultTheme.Layout>
      <template v-if="isArticle" #doc-before>
        <ArticleHero />
      </template>
      <template v-if="isArticle" #doc-after>
        <RelatedArticles />
      </template>
    </DefaultTheme.Layout>
  </div>
</template>
