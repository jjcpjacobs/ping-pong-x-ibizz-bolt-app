<script setup>
const props = defineProps({
  show: {
    type: Boolean,
    required: true
  },
  playerName: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['close', 'submit'])

function handleSubmit(event) {
  event.preventDefault()
  const player1Score = parseInt(event.target.player1Score.value, 10)
  const player2Score = parseInt(event.target.player2Score.value, 10)
  
  if (!isNaN(player1Score) && !isNaN(player2Score) && 
      player1Score >= 0 && player2Score >= 0) {
    emit('submit', { player1Score, player2Score })
    event.target.reset()
  }
}
</script>

<template>
  <div v-if="show" class="popup-overlay" @click.self="emit('close')">
    <div class="popup">
      <h3>Enter Match Score</h3>
      <form @submit="handleSubmit" class="popup-form">
        <div class="score-inputs">
          <div class="score-input-group">
            <label for="player1Score">Hans</label>
            <input 
              type="number"
              id="player1Score"
              name="player1Score"
              placeholder="Score"
              class="score-input"
              min="0"
              required
              autofocus
            >
          </div>
          <div class="score-separator">-</div>
          <div class="score-input-group">
            <label for="player2Score">Luc</label>
            <input 
              type="number"
              id="player2Score"
              name="player2Score"
              placeholder="Score"
              class="score-input"
              min="0"
              required
            >
          </div>
        </div>
        <div class="popup-buttons">
          <button type="button" @click="emit('close')" class="cancel-button">Cancel</button>
          <button type="submit" class="submit-button">Submit</button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.popup {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 90%;
  max-width: 400px;
}

.popup h3 {
  margin: 0 0 1.5rem 0;
  text-align: center;
  color: #2c3e50;
}

.popup-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.score-inputs {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}

.score-input-group {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.score-input-group label {
  font-weight: 500;
  color: #4b5563;
  text-align: center;
}

.score-separator {
  font-weight: 500;
  color: #6b7280;
  padding: 0 0.5rem;
  margin-top: 1.5rem;
  text-align: center;
}

.score-input {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1.1rem;
  text-align: center;
}

.popup-buttons {
  display: flex;
  gap: 1rem;
  justify-content: flex-end;
}

.cancel-button, .submit-button {
  padding: 0.6rem 1.2rem;
  border-radius: 4px;
  font-weight: 500;
  transition: opacity 0.2s;
}

.cancel-button {
  background: #e5e7eb;
  color: #4b5563;
}

.submit-button {
  background: #3b82f6;
  color: white;
}

.cancel-button:hover,
.submit-button:hover {
  opacity: 0.9;
}
</style>