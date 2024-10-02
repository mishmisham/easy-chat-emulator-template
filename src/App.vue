<template>
<div class="page">
  <chat-emulator-component/>
</div>
</template>

<script setup>
import ChatEmulatorComponent from '@/components/chat-emulator-component/ChatEmulatorComponent.vue'
</script>

<style lang="scss">
@import '@/styles/defaults.scss';

.page {
  @include flexbox();
  width: calc(100% - 48px);
  max-width: $maxBodyWidth;
  margin: auto;
}
</style>
