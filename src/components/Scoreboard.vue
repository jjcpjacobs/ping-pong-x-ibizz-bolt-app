<script setup>
import { ref, onMounted } from 'vue'
import ScorePopup from './ScorePopup.vue'
import PlayerStats from './PlayerStats.vue'

const initialPlayerState = {
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
}

const scores = ref({
  player1: { ...initialPlayerState },
  player2: { ...initialPlayerState }
})

const showPopup = ref(false)
const selectedPlayer = ref(null)

const lastUpdate = ref({
  day: 0,
  week: 0,
  month: 0,
  year: 0
})

function checkAndResetCounters() {
  const now = new Date()
  const currentDay = now.getDate()
  const currentMonth = now.getMonth()
  const currentYear = now.getFullYear()
  const currentWeek = Math.floor(now.getTime() / (7 * 24 * 60 * 60 * 1000))

  if (currentYear > lastUpdate.value.year) {
    scores.value.player1 = { ...initialPlayerState }
    scores.value.player2 = { ...initialPlayerState }
  } else if (currentMonth !== lastUpdate.value.month) {
    for (const player of ['player1', 'player2']) {
      scores.value[player].month = 0
      scores.value[player].week = 0
      scores.value[player].today = 0
      scores.value[player].scores.month = []
      scores.value[player].scores.week = []
      scores.value[player].scores.today = []
    }
  } else if (currentWeek !== lastUpdate.value.week) {
    for (const player of ['player1', 'player2']) {
      scores.value[player].week = 0
      scores.value[player].today = 0
      scores.value[player].scores.week = []
      scores.value[player].scores.today = []
    }
  } else if (currentDay !== lastUpdate.value.day) {
    for (const player of ['player1', 'player2']) {
      scores.value[player].today = 0
      scores.value[player].scores.today = []
    }
  }

  lastUpdate.value = {
    day: currentDay,
    week: currentWeek,
    month: currentMonth,
    year: currentYear
  }
}

function openScorePopup() {
  showPopup.value = true
}

function handleScoreSubmit({ player1Score, player2Score }) {
  checkAndResetCounters()

  // Determine the winner
  const winner = player1Score > player2Score ? 'player1' : 'player2'

  // Only update scores for the winner
  scores.value[winner].today++
  scores.value[winner].week++
  scores.value[winner].month++
  scores.value[winner].year++

  // Store both players' scores in the history
  scores.value.player1.scores.today.push(player1Score)
  scores.value.player1.scores.week.push(player1Score)
  scores.value.player1.scores.month.push(player1Score)
  scores.value.player1.scores.year.push(player1Score)

  scores.value.player2.scores.today.push(player2Score)
  scores.value.player2.scores.week.push(player2Score)
  scores.value.player2.scores.month.push(player2Score)
  scores.value.player2.scores.year.push(player2Score)

  try {
    localStorage.setItem('scoreboard', JSON.stringify({
      scores: scores.value,
      lastUpdate: lastUpdate.value
    }))
  } catch (error) {
    console.error('Error saving data:', error)
  }

  showPopup.value = false
}

onMounted(() => {
  try {
    const saved = localStorage.getItem('scoreboard')
    if (saved) {
      const data = JSON.parse(saved)
      if (data?.scores && data?.lastUpdate) {
        for (const player of ['player1', 'player2']) {
          scores.value[player] = {
            ...initialPlayerState,
            ...data.scores[player],
            scores: {
              today: data.scores[player]?.scores?.today || [],
              week: data.scores[player]?.scores?.week || [],
              month: data.scores[player]?.scores?.month || [],
              year: data.scores[player]?.scores?.year || []
            }
          }
        }
        lastUpdate.value = data.lastUpdate
      }
    }
  } catch (error) {
    console.error('Error loading saved data:', error)
    localStorage.removeItem('scoreboard')
  }
  
  checkAndResetCounters()
})
</script>

<template>
  <div class="scoreboard">
    <div class="buttons">
      <button class="player-button player1" @click="openScorePopup">
        Enter Match Score
      </button>
    </div>

    <div class="stats">
      <PlayerStats 
        name="Hans"
        :stats="scores.player1"
      />
      <PlayerStats 
        name="Luc"
        :stats="scores.player2"
      />
    </div>

    <ScorePopup
      :show="showPopup"
      :player-name="'Match'"
      @close="showPopup = false"
      @submit="handleScoreSubmit"
    />
  </div>
</template>

<style scoped>
.scoreboard {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.buttons {
  display: flex;
  gap: 2rem;
  margin-bottom: 2rem;
}

.player-button {
  flex: 1;
  font-size: 1.5rem;
  padding: 2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.1s, box-shadow 0.1s;
  background: #3b82f6;
  color: white;
}

.player-button:active {
  transform: scale(0.98);
}

.stats {
  display: flex;
  justify-content: space-around;
  gap: 2rem;
}
</style>