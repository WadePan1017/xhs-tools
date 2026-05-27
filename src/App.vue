<script setup>
import { ref } from 'vue'
import TitleGenerator from './components/TitleGenerator.vue'
import CopyRewriter from './components/CopyRewriter.vue'
import TagRecommender from './components/TagRecommender.vue'

const activeTab = ref('title')
const tabs = [
  { id: 'title', name: '标题生成器', icon: '✨' },
  { id: 'copy', name: '文案改写', icon: '  ' },
  { id: 'tags', name: '标签推荐', icon: '  ' },
]
</script>

<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white border-b border-gray-200 sticky top-0 z-50">
      <div class="max-w-4xl mx-auto px-4 py-4">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-3">
            <span class="text-2xl">  </span>
            <h1 class="text-xl font-bold text-gray-900">小红书工具箱</h1>
          </div>
          <span class="text-sm text-gray-500">AI 驱动创作效率</span>
        </div>
      </div>
    </header>

    <!-- Tab Navigation -->
    <div class="bg-white border-b border-gray-200">
      <div class="max-w-4xl mx-auto px-4">
        <nav class="flex gap-1 -mb-px">
          <button
            v-for="tab in tabs"
            :key="tab.id"
            @click="activeTab = tab.id"
            :class="[
              'px-4 py-3 text-sm font-medium border-b-2 transition-colors',
              activeTab === tab.id
                ? 'border-red-500 text-red-600'
                : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300'
            ]"
          >
            <span class="mr-2">{{ tab.icon }}</span>
            {{ tab.name }}
          </button>
        </nav>
      </div>
    </div>

    <!-- Content -->
    <main class="max-w-4xl mx-auto px-4 py-8">
      <TitleGenerator v-if="activeTab === 'title'" />
      <CopyRewriter v-else-if="activeTab === 'copy'" />
      <TagRecommender v-else-if="activeTab === 'tags'" />
    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-gray-200 mt-auto">
      <div class="max-w-4xl mx-auto px-4 py-6 text-center text-sm text-gray-500">
        <p>小红书工具箱 - 让创作更高效</p>
        <p class="mt-1">更多功能持续开发中...</p>
      </div>
    </footer>
  </div>
</template>
