<template>
  <div id="app">
    <h1>Bem-vindo ao Jogo das Portas!!!</h1>

    <div class="form">
      <div v-if="!started">
        <label for="portsAmount">Quantas portas?</label>
        <input
          type="number"
          id="portsAmount"
          v-model.number="portsAmount"
          min="1"
        />
      </div>

      <div v-if="!started">
        <label for="selectedPort">Qual é a porta premiada?</label>
        <input
          type="number"
          id="selectedPort"
          v-model.number="selectedPort"
          :max="portsAmount"
          min="1"
        />
      </div>

      <button v-if="!started" @click="validateAndStart">Iniciar</button>
      <button v-else @click="resetGame">Reiniciar</button>
    </div>

    <transition-group name="door-fade" tag="div" class="doors" v-if="started">
      <div v-for="i in portsAmount" :key="i">
        <Door :number="i" :hasGift="i === selectedPort" />
      </div>
    </transition-group>
  </div>
</template>

<script>
import Door from "./components/Door.vue";
// 📌 ajuste o caminho se necessário
export default {
  name: "App",
  components: { Door },
  data() {
    return {
      started: false,
      portsAmount: 3,
      selectedPort: null
    };
  },
  methods: {
    validateAndStart() {
      if (
        this.portsAmount < 1 ||
        this.selectedPort < 1 ||
        this.selectedPort > this.portsAmount
      ) {
        alert("Escolha uma porta premiada válida!");
        return;
      }

      // 🔊 Fanfarra
      const fanfare = new Audio(require("@/assets/sounds/fanfare.mp3"));
      fanfare.volume = 0.8;
      fanfare.play();

      this.started = true;
    },

    resetGame() {
      this.started = false;
      this.selectedPort = null;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  color: #fff;
  background: linear-gradient(to top, #240b36, #c31432) no-repeat;
  height: 100vh;
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  width: 580px;
  border: 1px solid #000;
  background-color: #0004;
  padding: 20px;
  margin: 40px 0;
  border-radius: 15px;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  margin-bottom: 40px;
}

input {
  padding: 8px;
  font-size: 1rem;
  border-radius: 6px;
  width: 80px;
}

button {
  background-color: gold;
  color: #000;
  font-weight: bold;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: orange;
}

.doors {
  display: flex;
  gap: 20px;
}

/* ✨ Animação de entrada das portas */
.door-fade-enter-active {
  transition: all 0.5s ease;
}
.door-fade-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.door-fade-enter-to {
  opacity: 1;
  transform: translateY(0);
}
</style>