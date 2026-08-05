<template>
  <div
    class="card"
    :style="{ '--tone': city.temp >= 25 ? 'var(--hot)' : 'var(--cool)' }"
    tabindex="0"
    @click="$emit('select-card', city.name)"
    @keydown.enter="$emit('select-card', city.name)"
  >
    <div class="card-top">
      <span class="city-name">{{ city.name }}</span>
      <span class="status-icon">{{ statusIcon }}</span>
    </div>

    <div class="temp">{{ city.temp }}°C</div>
    <div class="status-text">{{ city.status }}</div>

    <span v-if="city.temp >= 25" class="badge hot">🔥더움(25도이상)</span>
    <span v-else class="badge cool">❄️선선함(25도미만)</span>

    <div class="card-bottom">
      <button class="detail-btn" @click.stop="$emit('click-detail', city.name, city.status)">
        상세보기
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  city: {
    type: Object,
    required: true,
  },
})

defineEmits(['select-card', 'click-detail'])

const statusIcon = computed(() => {
  if (props.city.status === '맑음') return '☀️'
  if (props.city.status === '비') return '🌧️'
  if (props.city.status === '구름') return '⛅'
  return '🌡️'
})
</script>

<style scoped>
.card {
  position: relative;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 18px 18px 16px;
  cursor: pointer;
  overflow: hidden;
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}
.card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: var(--tone, var(--muted));
}
.card:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 24px -12px color-mix(in srgb, var(--tone, var(--ink)) 45%, transparent);
}
.card:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
@media (prefers-reduced-motion: reduce) {
  .card {
    transition: none;
  }
  .card:hover {
    transform: none;
  }
}

.card-top {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 8px;
}
.city-name {
  font-size: 1.05rem;
  font-weight: 700;
}
.status-icon {
  font-size: 1.1rem;
}

.temp {
  margin-top: 10px;
  font-family: ui-monospace, 'SFMono-Regular', 'Cascadia Code', Consolas, monospace;
  font-variant-numeric: tabular-nums;
  font-size: 2.1rem;
  font-weight: 700;
  line-height: 1;
}
.status-text {
  margin-top: 4px;
  color: var(--muted);
  font-size: 0.88rem;
}

.badge {
  display: inline-block;
  margin-top: 12px;
  padding: 4px 10px;
  border-radius: 999px;
  font-size: 0.78rem;
  font-weight: 700;
}
.badge.hot {
  background: var(--hot-bg);
  color: var(--hot);
}
.badge.cool {
  background: var(--cool-bg);
  color: var(--cool);
}

.card-bottom {
  margin-top: 14px;
  display: flex;
  justify-content: flex-end;
}
.detail-btn {
  border: 1px solid var(--border);
  background: var(--surface-2);
  color: var(--ink);
  font-size: 0.82rem;
  font-weight: 600;
  padding: 7px 12px;
  border-radius: 8px;
  cursor: pointer;
  font-family: inherit;
}
.detail-btn:hover {
  background: var(--border);
}
.detail-btn:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
</style>
