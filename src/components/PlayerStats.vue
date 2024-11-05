<script setup>
const props = defineProps({
  name: {
    type: String,
    required: true
  },
  stats: {
    type: Object,
    required: true,
    default: () => ({
      today: 0,
      week: 0,
      month: 0,
      year: 0,
      scores: {
        today: [],
        week: [],
        month: [],
        year: []
      }
    })
  }
})

function getTotal(scores = []) {
  return scores.reduce((sum, score) => sum + score, 0)
}
</script>

<template>
  <div class="stat-column">
    <h2>{{ name }}</h2>
    <div class="stat-item">
      <div>Today: {{ stats?.today || 0 }}</div>
      <div class="total">Total: {{ getTotal(stats?.scores?.today) }}</div>
    </div>
    <div class="stat-item">
      <div>This Week: {{ stats?.week || 0 }}</div>
      <div class="total">Total: {{ getTotal(stats?.scores?.week) }}</div>
    </div>
    <div class="stat-item">
      <div>This Month: {{ stats?.month || 0 }}</div>
      <div class="total">Total: {{ getTotal(stats?.scores?.month) }}</div>
    </div>
    <div class="stat-item">
      <div>This Year: {{ stats?.year || 0 }}</div>
      <div class="total">Total: {{ getTotal(stats?.scores?.year) }}</div>
    </div>
  </div>
</template>

<style scoped>
.stat-column {
  flex: 1;
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.stat-column h2 {
  margin: 0 0 1rem 0;
  color: #2c3e50;
  text-align: center;
}

.stat-item {
  padding: 0.5rem;
  margin: 0.5rem 0;
  background: #f8f9fa;
  border-radius: 4px;
  text-align: center;
  font-size: 1.1rem;
}

.total {
  font-size: 0.9rem;
  color: #666;
  margin-top: 0.2rem;
}
</style>