<template>
  <div class="page">
    <header>
      <div class="eyebrow">VUE 실습 · 컴포넌트 분리 (props / emits / slot)</div>
      <h1>지역별 날씨 현황판</h1>
      <div class="sub">카드를 클릭하면 도시가 선택되고, [상세보기]는 클릭이 카드로 번지지 않아요.</div>
    </header>

    <!-- 4. 이벤트 및 수식어: 카드 클릭 시 상태바 표시 -->
    <div class="board">
      <span class="dot"></span>
      <span v-if="statusMessage">{{ statusMessage }}</span>
      <span v-else class="placeholder">카드를 클릭해 도시를 선택해보세요.</span>
    </div>

    <!-- BaseDashboardCard의 slot으로 주입되지만, SearchBar는 WeatherParent 스코프에서
         컴파일되므로 여기서 searchText/onSearchInput과 직접 바인딩할 수 있다. -->
    <BaseDashboardCard>
      <SearchBar :search-text="searchText" @update-query="onSearchInput" />
    </BaseDashboardCard>

    <BaseDashboardCard>
      <div class="grid">
        <WeatherCard
          v-for="city in filteredList"
          :key="city.id"
          :city="city"
          @select-card="selectCity"
          @click-detail="showDetail"
        />
        <p v-if="filteredList.length === 0" class="empty">검색 결과가 없습니다.</p>
      </div>
    </BaseDashboardCard>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import BaseDashboardCard from './BaseDashboardCard.vue'
import SearchBar from './SearchBar.vue'
import WeatherCard from './WeatherCard.vue'

// 모든 반응형 데이터는 WeatherParent가 유지한다.
const weatherList = ref([
  { id: 'city_01', name: '서울', temp: 28, status: '맑음' },
  { id: 'city_02', name: '수원', temp: 24, status: '비' },
  { id: 'city_03', name: '부산', temp: 26, status: '구름' },
])

const searchText = ref('')
const statusMessage = ref('')

function onSearchInput(value) {
  searchText.value = value
}

const filteredList = computed(() => {
  if (!searchText.value) return weatherList.value
  return weatherList.value.filter((city) => city.name.includes(searchText.value))
})

function selectCity(name) {
  statusMessage.value = `${name}이 선택되었습니다.`
}

function showDetail(cityName, status) {
  window.alert(`${cityName}의 현재 날씨는 [${status}] 상태입니다.`)
}
</script>

<style scoped>
.page {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 24px 64px;
  display: flex;
  flex-direction: column;
  gap: 22px;
}
.eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.14em;
  color: var(--muted);
  font-weight: 600;
}
h1 {
  margin: 2px 0 0;
  font-size: 1.7rem;
  font-weight: 800;
  text-wrap: balance;
}
.sub {
  color: var(--muted);
  font-size: 0.92rem;
  margin-top: 2px;
}

.board {
  background: var(--board-bg);
  color: var(--board-ink);
  border-radius: 10px;
  padding: 14px 18px;
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: ui-monospace, 'SFMono-Regular', 'Cascadia Code', Consolas, monospace;
  font-size: 0.92rem;
}
.board .dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--board-accent);
  flex-shrink: 0;
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--board-accent) 25%, transparent);
}
.board .placeholder {
  color: var(--board-muted);
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
.empty {
  color: var(--muted);
  font-size: 0.9rem;
  padding: 12px 4px;
  margin: 0;
}
</style>
