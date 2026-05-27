<script setup>
import { ref } from 'vue'

const keyword = ref('')
const results = ref([])
const loading = ref(false)

const titleTemplates = [
  { prefix: '我用AI做了', suffix: '，效果绝了' },
  { prefix: '普通人也能', suffix: '，方法分享' },
  { prefix: '这个', suffix: '真的太好用了' },
  { prefix: '后悔没早点知道的', suffix: '技巧' },
  { prefix: '保姆级', suffix: '教程来了' },
  { prefix: '被问爆的', suffix: '终于整理好了' },
  { prefix: '一个人', suffix: '是什么体验' },
  { prefix: 'AI帮我', suffix: '，省了3天时间' },
  { prefix: '新手必看', suffix: '避坑指南' },
  { prefix: '偷偷', suffix: '，惊艳所有人' },
]

const emojis = ['✨', '  ', '  ', '  ', '  ', '  ', ' ', '  ', '  ', '  ']

function generate() {
  if (!keyword.value.trim()) return
  loading.value = true

  setTimeout(() => {
    const shuffled = [...titleTemplates].sort(() => Math.random() - 0.5)
    results.value = shuffled.slice(0, 6).map((t, i) => ({
      text: `${t.prefix}${keyword.value}${t.suffix}`,
      emoji: emojis[i % emojis.length],
    }))
    loading.value = false
  }, 500)
}

function copyTitle(text) {
  navigator.clipboard.writeText(text)
}
</script>

<template>
  <div class="space-y-6">
    <div class="bg-white rounded-xl p-6 border border-gray-200">
      <h2 class="text-lg font-semibold text-gray-900 mb-2">  标题生成器</h2>
      <p class="text-sm text-gray-500 mb-4">输入关键词，一键生成爆款标题</p>

      <div class="flex gap-3">
        <input
          v-model="keyword"
          type="text"
          placeholder="输入关键词，如：摄影、穿搭、美食、旅行..."
          class="flex-1 px-4 py-3 border border-gray-200 rounded-lg text-sm focus:outline-none focus:border-red-500"
          @keyup.enter="generate"
        />
        <button
          @click="generate"
          :disabled="loading || !keyword.trim()"
          class="px-6 py-3 bg-red-500 text-white rounded-lg text-sm font-medium hover:bg-red-600 disabled:bg-gray-300 disabled:cursor-not-allowed transition-colors"
        >
          {{ loading ? '生成中...' : '生成标题' }}
        </button>
      </div>
    </div>

    <!-- Results -->
    <div v-if="results.length" class="space-y-3">
      <div
        v-for="(item, index) in results"
        :key="index"
        class="bg-white rounded-xl p-4 border border-gray-200 flex items-center justify-between group hover:border-red-300 transition-colors"
      >
        <div class="flex items-center gap-3">
          <span class="text-xl">{{ item.emoji }}</span>
          <span class="text-sm text-gray-800">{{ item.text }}</span>
        </div>
        <button
          @click="copyTitle(item.text)"
          class="px-3 py-1.5 text-xs text-gray-500 hover:text-red-500 hover:bg-red-50 rounded-md transition-colors opacity-0 group-hover:opacity-100"
        >
          复制
        </button>
      </div>
    </div>

    <div v-else class="text-center py-12 text-gray-400">
      <p class="text-4xl mb-3">  </p>
      <p class="text-sm">输入关键词，开始生成爆款标题</p>
    </div>
  </div>
</template>
