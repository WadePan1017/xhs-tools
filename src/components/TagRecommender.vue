<script setup>
import { ref } from 'vue'

const topic = ref('')
const tags = ref([])
const loading = ref(false)

const tagDatabase = {
  '摄影': ['摄影', '摄影教程', '拍照技巧', '人像摄影', '风光摄影', '手机摄影', '摄影后期', '写真', '约拍', '摄影师'],
  '穿搭': ['穿搭', '每日穿搭', '穿搭分享', 'ootd', '穿搭推荐', '显瘦穿搭', '通勤穿搭', '春季穿搭', '时尚', '搭配'],
  '美食': ['美食', '美食分享', '美食探店', '家常菜', '减脂餐', '甜品', '烘焙', '料理', '好吃', '美食教程'],
  '旅行': ['旅行', '旅行攻略', '旅游', '打卡', '小众旅行', '周末去哪', '旅行日记', '风景', '旅行必备', '说走就走'],
  '护肤': ['护肤', '护肤分享', '护肤品推荐', '干皮护肤', '油皮护肤', '美白', '抗老', '平价护肤', '成分党', '护肤步骤'],
  '健身': ['健身', '健身打卡', '减脂', '增肌', '居家健身', '健身教程', '运动', '减肥', '塑形', '健身餐'],
  '学习': ['学习', '学习方法', '效率', '自律', '考研', '考公', '英语学习', '笔记', '学习分享', '成长'],
  '副业': ['副业', '赚钱', '搞钱', '兼职', '自由职业', '独立开发', '接单', '被动收入', '理财', '搞钱攻略'],
}

function recommend() {
  if (!topic.value.trim()) return
  loading.value = true

  setTimeout(() => {
    const matchedTags = []
    for (const [key, value] of Object.entries(tagDatabase)) {
      if (topic.value.includes(key) || key.includes(topic.value)) {
        matchedTags.push(...value)
      }
    }
    if (matchedTags.length === 0) {
      matchedTags.push(
        topic.value,
        `${topic.value}分享`,
        `${topic.value}推荐`,
        `${topic.value}教程`,
        '日常',
        '记录生活'
      )
    }
    tags.value = [...new Set(matchedTags)].slice(0, 15)
    loading.value = false
  }, 600)
}

function copyTag(tag) {
  navigator.clipboard.writeText(`#${tag}`)
}

function copyAll() {
  const allTags = tags.value.map(t => `#${t}`).join(' ')
  navigator.clipboard.writeText(allTags)
}
</script>

<template>
  <div class="space-y-6">
    <div class="bg-white rounded-xl p-6 border border-gray-200">
      <h2 class="text-lg font-semibold text-gray-900 mb-2">  标签推荐</h2>
      <p class="text-sm text-gray-500 mb-4">根据内容主题推荐热门标签</p>

      <div class="flex gap-3">
        <input
          v-model="topic"
          type="text"
          placeholder="输入内容主题，如：摄影、美食、穿搭..."
          class="flex-1 px-4 py-3 border border-gray-200 rounded-lg text-sm focus:outline-none focus:border-red-500"
          @keyup.enter="recommend"
        />
        <button
          @click="recommend"
          :disabled="loading || !topic.trim()"
          class="px-6 py-3 bg-red-500 text-white rounded-lg text-sm font-medium hover:bg-red-600 disabled:bg-gray-300 disabled:cursor-not-allowed transition-colors"
        >
          {{ loading ? '推荐中...' : '推荐标签' }}
        </button>
      </div>
    </div>

    <!-- Results -->
    <div v-if="tags.length" class="bg-white rounded-xl p-6 border border-gray-200">
      <div class="flex items-center justify-between mb-4">
        <h3 class="text-sm font-medium text-gray-700">推荐标签</h3>
        <button
          @click="copyAll"
          class="px-3 py-1.5 text-xs text-gray-500 hover:text-red-500 hover:bg-red-50 rounded-md transition-colors"
        >
          复制全部
        </button>
      </div>

      <div class="flex flex-wrap gap-2">
        <button
          v-for="tag in tags"
          :key="tag"
          @click="copyTag(tag)"
          class="px-4 py-2 bg-red-50 text-red-600 rounded-full text-sm hover:bg-red-100 transition-colors cursor-pointer"
        >
          #{{ tag }}
        </button>
      </div>

      <p class="text-xs text-gray-400 mt-4">点击标签可单独复制，建议选择 5-10 个标签</p>
    </div>

    <div v-else class="text-center py-12 text-gray-400">
      <p class="text-4xl mb-3">  </p>
      <p class="text-sm">输入内容主题，获取热门标签推荐</p>
    </div>
  </div>
</template>
