<template>
  <div class="search-row">
    <label for="citySearch">도시 이름 검색</label>
    <div class="search-field">
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
        <circle cx="7" cy="7" r="5" stroke="currentColor" stroke-width="1.5" />
        <path d="M11 11L14.5 14.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" />
      </svg>
      <input
        id="citySearch"
        type="text"
        placeholder="예: 서울, 부산"
        :value="searchText"
        @input="handleInput"
      />
    </div>
    <div class="search-echo" v-if="searchText">
      입력한 도시명: <b>{{ searchText }}</b>
    </div>
  </div>
</template>

<script setup>
defineProps({
  searchText: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['update-query'])

function handleInput(event) {
  emit('update-query', event.target.value)
}
</script>

<style scoped>
.search-row {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.search-row label {
  font-size: 0.8rem;
  color: var(--muted);
  font-weight: 600;
}
.search-field {
  display: flex;
  align-items: center;
  gap: 8px;
  background: var(--surface-2);
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 10px 16px;
  max-width: 340px;
}
.search-field svg {
  flex-shrink: 0;
  color: var(--muted);
}
.search-field input {
  border: none;
  outline: none;
  background: transparent;
  color: var(--ink);
  font-size: 0.95rem;
  font-family: inherit;
  width: 100%;
}
.search-echo {
  font-size: 0.85rem;
  color: var(--muted);
}
.search-echo b {
  color: var(--accent);
  font-weight: 700;
}
</style>
