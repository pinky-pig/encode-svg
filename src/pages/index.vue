<script setup lang="ts">
import Background from '~/components/ui/Background.vue'

// 点击 Preview 更换背景色
const isRerenderFlag = ref(0)
</script>

<template>
  <Background v-model="isRerenderFlag" />
  <ConfettiCanvas />

  <Suspense>
    <div class="fixed bottom-0 left-0 right-0 top-0 h-full w-full flex flex-col overflow-x-hidden overflow-y-auto">
      <Header />
      <div class="main-content-layout flex flex-1 justify-center items-center">
        <MainInput />
      </div>
    </div>
  </Suspense>
</template>

<style scoped>
.main-content-layout {
  flex-direction: row;
}
@media screen and (max-width: 768px) {
  .main-content-layout {
    flex-direction: column;
    align-items: center;
  }
  .footer {
    position: unset;
  }
  .footer .footer-placeholder {
    height: 0 !important;
    width: 0 !important;
  }
}
</style>
