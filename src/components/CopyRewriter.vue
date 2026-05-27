<script setup>
import { ref } from 'vue'

const originalText = ref('')
const rewrittenText = ref('')
const loading = ref(false)
const mode = ref('casual')

const modes = [
  { id: 'casual', name: '轻松口语', desc: '更接地气的表达' },
  { id: 'professional', name: '专业干货', desc: '更有说服力' },
  { id: 'emotional', name: '情感共鸣', desc: '更有感染力' },
  { id: 'humorous', name: '幽默有趣', desc: '更轻松活泼' },
]

function rewrite() {
  if (!originalText.value.trim()) return
  loading.value = true

  setTimeout(() => {
    let text = originalText.value
    if (mode.value === 'casual') {
      text = text.replace(/。/g, '～').replace(/！/g, '！')
      rewrittenText.value = `姐妹们！${text}`
    } else if (mode.value === 'professional') {
      rewrittenText.value = `干货分享：${text}`
    } else if (mode.value === 'emotional') {
      rewrittenText.value = `有没有人跟我一样...${text}`
    } else {
      rewrittenText.value = `哈哈 ${text}（doge）`
    }
    loading.value = false
  }, 800)
}

function copyText() {
  navigator.clipboard.writeText(rewrittenText.value)
}
</script>

<template>
  <div class="space-y-6">
    <div class="bg-white rounded-xl p-6 border border-gray-200">
      <h2 class="text-lg font-semibold text-gray-900 mb-2">✏️ 文案改写</h2>
      <p class="text-sm text-gray-500 mb-4">一键改写文案，避免重复，提升原创度</p>

      <textarea
        v-model="originalText"
        placeholder="粘贴你的文案内容..."
        rows="5"
        class="w-full px-4 py-3 border border-gray-200 rounded-lg text-sm focus:outline-none focus:border-red-500 resize-none"
      ></textarea>

      <!-- Mode Selection -->
      <div class="flex flex-wrap gap-2 mt-4 mb-4">
        <button
          v-for="m in modes"
          :key="m.id"
          @click="mode = m.id"
          :class="[
            'px-4 py-2 rounded-lg text-sm transition-colors',
            mode === m.id
              ? 'bg-red-500 text-white'
              : 'bg-gray-100 text-gray-600 hover:bg-gray-200'
          ]"
        >
          {{ m.name }}
        </button>
      </div>

      <button
        @click="rewrite"
        :disabled="loading || !originalText.trim()"
        class="w-full px-6 py-3 bg-red-500 text-white rounded-lg text-sm font-medium hover:bg-red-600 disabled:bg-gray-300 disabled:cursor-not-allowed transition-colors"
      >
        {{ loading ? '改写中...' : '一键改写' }}
      </button>
    </div>

    <!-- Result -->
    <div v-if="rewrittenText" class="bg-white rounded-xl p-6 border border-gray-200">
      <div class="flex items-center justify-between mb-3">
        <h3 class="text-sm font-medium text-gray-700">改写结果</h3>
        <button
          @click="copyText"
          class="px-3 py-1.5 text-xs text-gray-500 hover:text-red-500 hover:bg-red-50 rounded-md transition-colors"
        >
          复制
        </button>
      </div>
      <div class="bg-gray-50 rounded-lg p-4 text-sm text-gray-800 whitespace-pre-wrap leading-relaxed">
        {{ rewrittenText }}
      </div>
    </div>
  </div>
</template>
