<script setup lang="ts">
import { Toaster, toast } from 'vue-sonner'
import { getResults } from '~/utils/encodeSvg'

const originalSvg = ref('')

const externalQuotesValue = ref<'double' | 'single'>('double')

function handleSwitchQuote(quote: 'double' | 'single') {
  externalQuotesValue.value = quote
}

const encodeSvg = computed(() => {
  return getResults(originalSvg.value, externalQuotesValue.value).encoded
})

const cssSvg = computed(() => {
  return getResults(originalSvg.value, externalQuotesValue.value).resultCss
})

function handleSetExample() {
  originalSvg.value = `<svg>
      <circle r="50" cx="50" cy="50" fill="tomato"/>
      <circle r="41" cx="47" cy="50" fill="orange"/>
      <circle r="33" cx="48" cy="53" fill="gold"/>
      <circle r="25" cx="49" cy="51" fill="yellowgreen"/>
      <circle r="17" cx="52" cy="50" fill="lightseagreen"/>
      <circle r="9" cx="55" cy="48" fill="teal"/>
    </svg>
  `
}

function handleCopy(item: 'encode' | 'css') {
  if (item === 'encode') {
    navigator.clipboard.writeText(encodeSvg.value)
    toast.success('Copy success')

    const textarea = document.getElementById('encode-svg')
    if (textarea)
      (textarea as HTMLTextAreaElement).select()
  }

  if (item === 'css') {
    navigator.clipboard.writeText(cssSvg.value)
    toast.success('Copy success')

    const textarea = document.getElementById('svg-in-css')
    if (textarea)
      (textarea as HTMLTextAreaElement).select()
  }
}

const previewBackgrounds = ref(['#fff', '#ffdd65', '#7e41ff'])
const currentPreviewBackground = ref(previewBackgrounds.value[0])

function handleSetPreviewBackground(item: typeof previewBackgrounds.value[number]) {
  currentPreviewBackground.value = item
}
</script>

<template>
  <div class="grid grid-cols-2 gap-6 text-[#fff]">
    <div class="quote">
      <div class="quote-btn">
        <span>External quotes : </span>
        <span :class="externalQuotesValue === 'single' ? 'text-[#ffdd65]' : 'text-blue underline underline-blue underline-offset-8 underline-dashed'" class="cursor-pointer" @click="handleSwitchQuote('single')">Single</span>
        <span> / </span>
        <span :class="externalQuotesValue === 'double' ? 'text-[#ffdd65]' : 'text-blue underline underline-blue underline-offset-8 underline-dashed'" class="cursor-pointer" @click="handleSwitchQuote('double')">Double</span>
      </div>
    </div>
    <!-- 输入的 SVG -->
    <div class="card relative">
      <div class="card-title">
        <span for="insert-svg">Insert SVG</span>
        <span class="card-title-btn" @click="handleSetExample">Demo</span>
      </div>

      <textarea id="insert-svg" v-model="originalSvg" class="textarea" />

      <StickerArrow class="absolute top-100px -left-180px" />
    </div>

    <!-- 预览 -->
    <div
      class="card !bg-[#fff] !text-black !border-[#212132]"
      :style="{ backgroundColor: `${currentPreviewBackground} !important` }"
    >
      <div class="card-title">
        <span for="svg-in-css">Preview</span>

        <div class="card-title-preview">
          <span
            v-for="item in previewBackgrounds"
            :key="item"
            class="card-title-preview-btn "
            :style="{
              backgroundColor: `${item} !important`,
              border: `2px solid ${currentPreviewBackground === item ? '#77768559' : '#f1f5f7'}`,
            }"
            @click="handleSetPreviewBackground(item)"
          />
        </div>
      </div>

      <div class="textarea !border-dashed">
        <div class="preview-demo" :style="cssSvg" />
      </div>
    </div>

    <!-- 编码 -->
    <div class="card">
      <div class="card-title">
        <span for="encode-svg">Take encoded</span>
        <span class="card-title-btn" @click="handleCopy('encode')">Copy</span>
      </div>

      <textarea id="encode-svg" v-model="encodeSvg" readonly class="textarea" />
    </div>

    <!-- 在 css 中 -->
    <div class="card">
      <div class="card-title">
        <span for="svg-in-css">Ready for CSS</span>
        <span class="card-title-btn" @click="handleCopy('css')">Copy</span>
      </div>

      <textarea id="svg-in-css" v-model="cssSvg" readonly class="textarea" />
    </div>

    <Toaster
      position="top-center"
      :toast-options="{
        className: 'my-toast',
        descriptionClassName: 'my-toast-description',
      }"
      close-button
    />
  </div>
</template>

<style>
.my-toast {
  background: #7e41ff !important;
  color: white !important;
  border: 0px solid #ffdd65 !important;
  font-weight: 800 !important;
}
</style>

<style scoped>
.quote {
  display: flex;
  flex-direction: row;
  justify-content: start;
  align-items: center;
  grid-column: span 2;
}
.quote-btn {
  background-color: rgb(33, 33, 50);
  color: #fff;
  padding: 4px 20px;
  font-size: 18px;
  border: 1px solid #212132;
  border-radius: 10px;
  /* background-color: #7e41ff; */
  box-shadow: 0 4px 1.5em 0 rgba(33, 33, 50, 0.4);
}
.card {
  border: 1px solid #9d7bff;
  background: #212132;
  border-radius: 20px;
  padding: 20px 20px 10px;
  height: fit-content;
}
.card-title {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 20px;
}
.card-title-btn {
  background-color: rgb(33, 33, 50);
  color: #ffdd65;
  padding: 4px 20px;
  border: 2px solid hsla(0, 0%, 100%, 0.1);
  border-radius: 20px;
  cursor: pointer;
  font-size: 12px;
  font-weight: 600;
  font-style: italic;
}
.card-title-btn:hover {
  background-color: rgb(255, 221, 101);
  color: black;
  font-style: unset;
}

.card-title-preview-btn {
  background-color: rgb(33, 33, 50);
  color: rgb(255, 221, 101);
  padding: 4px 20px;
  border: 2px solid #f1f5f7;
  border-radius: 20px;
  cursor: pointer;
  font-size: 12px;
  font-weight: 600;
  font-style: italic;
  margin-right: 6px;
}
.card-title-preview-btn:hover {
  border: 2px solid rgb(255, 221, 101);
}
.textarea {
  background: transparent;
  border: 1px solid #9d7bff2b;
  outline: none;
  padding: 0.5rem;
  border-radius: 0.375rem;
  resize: vertical;
  height: 200px;
  width: 340px;
  min-height: 100px;

  font-size: 14px;
  font-family: monospace;
}
.preview-demo {
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
}
</style>
