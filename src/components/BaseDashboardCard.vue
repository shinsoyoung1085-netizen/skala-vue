<template>
  <div class="dashboard-card">
    <slot />
  </div>
</template>

<script setup>
// 검색 박스와 리스트 박스가 공유하는 공통 카드 껍데기. 내용은 부모가 slot으로 주입한다.
</script>

<style scoped>
.dashboard-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 18px;
}
</style>
