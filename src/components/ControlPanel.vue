<template>
  <div class="controls">
    <input
      type="number"
      v-model.number="localTimeLimit"
      placeholder="Time (ms)"
      class="input"
      :min="minTimeLimit"
      :max="maxTimeLimit"
    />
    <button @click="startGame" class="btn">Начать</button>
    <div class="score">
      Игрок: {{ playerScore }} | Компьютер: {{ computerScore }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    playerScore: {
      type: Number,
      required: true,
    },
    computerScore: {
      type: Number,
      required: true,
    },
    timeLimit: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      localTimeLimit: this.timeLimit,
      minTimeLimit: 100, 
      maxTimeLimit: 10000 
    };
  },
  watch: {
    timeLimit(newValue) {
      this.localTimeLimit = newValue;
    },
  },
  methods: {
    startGame() {
      if (this.localTimeLimit < this.minTimeLimit || this.localTimeLimit > this.maxTimeLimit) {
        alert(`Пожалуйста, введите значение между ${this.minTimeLimit} и ${this.maxTimeLimit} миллисекунд.`);
        this.localTimeLimit = '';
        return;
      }
      this.$emit("start-game", this.localTimeLimit);
    },
  },
};
</script>

<style scoped>
.controls {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input {
  padding: 10px;
  width: 200px;
  margin-bottom: 10px;
  font-size: 16px;
  border: 2px solid #ccc;
  border-radius: 5px;
  outline: none;
  transition: border-color 0.3s;
}

.input:focus {
  border-color: #007bff;
}

.score {
  margin-top: 10px;
  font-size: 18px;
  font-weight: bold;
}
</style>
