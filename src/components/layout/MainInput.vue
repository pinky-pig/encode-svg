<script setup lang="ts">
import { getResults } from '~/utils/encodeSvg'

const originalSvg = ref('aaa')

const encodeSvg = computed(() => {
  return getResults(originalSvg.value).encoded
})

const cssSvg = computed(() => {
  return getResults(originalSvg.value).resultCss
})
</script>

<template>
  <div class="grid grid-cols-2 gap-6 text-[#A4B2C1]">
    <!-- 输入的 SVG -->
    <div>
      <div class="flex flex-row justify-between">
        <label for="insert-svg">插入 SVG:</label>
        <span>demo</span>
      </div>

      <textarea id="insert-svg" v-model="originalSvg" class="textarea" />
    </div>

    <!-- 编码 -->
    <div>
      <div class="flex flex-row justify-between">
        <label for="encode-svg">插入 SVG:</label>
        <span>demo</span>
      </div>

      <textarea id="encode-svg" v-model="encodeSvg" readonly class="textarea" />
    </div>

    <!-- 在 css 中 -->
    <div>
      <div class="flex flex-row justify-between">
        <label for="svg-in-css">插入 SVG:</label>
        <span>demo</span>
      </div>

      <textarea id="svg-in-css" v-model="cssSvg" readonly class="textarea" />
    </div>

    <div>
      <div class="flex flex-row justify-between">
        <label for="svg-in-css">预览</label>
        <span>demo</span>
      </div>

      <div class="textarea">
        <div class="preview-demo" :style="cssSvg" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.textarea {
  background: #1d2026;
  border: 1px solid #4e5566;
  outline: none;
  padding: 0.5rem;
  border-radius: 0.375rem;
  resize: vertical;
  height: 180px;
  width: 340px;
}
.preview-demo {
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
}
</style>
