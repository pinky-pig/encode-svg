<script setup lang="ts">

</script>

<template>
  <ConfettiCanvas />

  <Suspense>
    <div class="fixed bottom-0 left-0 right-0 top-0 h-full w-full flex flex-col overflow-x-hidden overflow-y-auto">
      <Sticker class="fixed top-80vh right-10vw w-143px h-65px -z-1" />

      <Header />
      <div class="main-content-layout flex flex-col flex-1 justify-center items-center py-40px">
        <MainInput />
      </div>
    </div>
  </Suspense>
</template>

<style scoped>
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
