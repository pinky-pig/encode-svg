<script setup lang="ts">
import { getResults } from '~/utils/encodeSvg'

const originalSvg = ref('')
const externalQuotesValue = ref<'double' | 'single'>('double')

const encodeSvg = computed(() => {
  return getResults(originalSvg.value).encoded
})

const cssSvg = computed(() => {
  return getResults(originalSvg.value).resultCss
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
</script>

<template>
  <div class="grid grid-cols-2 gap-6 text-[#fff]">
    <div class="quote">
      <div class="quote-btn">
        <span>External quotes : </span>
        <label for="switch">Toggle</label>
        <input type="checkbox" id="switch" v-model="externalQuotesValue" />
      </div>
    </div>
    <!-- 输入的 SVG -->
    <div class="card">
      <div class="card-title">
        <span for="insert-svg">Insert SVG</span>
        <span class="card-title-btn" @click="handleSetExample">Demo</span>
      </div>

      <textarea id="insert-svg" v-model="originalSvg" class="textarea" />
    </div>

    <!-- 预览 -->
    <div class="card !bg-[#fff] !text-black !border-[#212132]">
      <div class="card-title">
        <span for="svg-in-css">Preview</span>

        <div class="card-title-preview">
          <span class="card-title-preview-btn !bg-white" />
          <span class="card-title-preview-btn !bg-gray" />
          <span class="card-title-preview-btn !bg-black" />
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
      </div>

      <textarea id="encode-svg" v-model="encodeSvg" readonly class="textarea" />
    </div>

    <!-- 在 css 中 -->
    <div class="card">
      <div class="card-title">
        <span for="svg-in-css">Ready for CSS</span>
      </div>

      <textarea id="svg-in-css" v-model="cssSvg" readonly class="textarea" />
    </div>
  </div>
</template>

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
  cursor: pointer;
  font-size: 18px;
  font-weight: 600;

  border: 1px solid #212132;
  border-radius: 10px;
  background-color: #7e41ff;
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
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 20px;
}
.card-title-btn {
  background-color: rgb(33, 33, 50);
  color: rgb(255, 221, 101);
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
  border: 2px solid rgb(241, 245, 247);
  border-radius: 20px;
  cursor: pointer;
  font-size: 12px;
  font-weight: 600;
  font-style: italic;
}
.card-title-preview-btn:hover {
  border: 2px solid rgb(255, 221, 101);
}
.textarea {
  background: transparent;
  border: 1px solid #4e5566;
  outline: none;
  padding: 0.5rem;
  border-radius: 0.375rem;
  resize: vertical;
  height: 200px;
  width: 340px;
  min-height: 100px;
}
.preview-demo {
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
}

input[type=checkbox]{
	height: 0;
	width: 0;
	visibility: hidden;
}

label {
	cursor: pointer;
	text-indent: -9999px;
	width: 200px;
	height: 100px;
	background: grey;
	display: block;
	border-radius: 100px;
	position: relative;
}

label:after {
	content: '';
	position: absolute;
	top: 5px;
	left: 5px;
	width: 90px;
	height: 90px;
	background: #fff;
	border-radius: 90px;
	transition: 0.3s;
}

input:checked + label {
	background: #bada55;
}

input:checked + label:after {
	left: calc(100% - 5px);
	transform: translateX(-100%);
}

label:active:after {
	width: 130px;
}

</style>
