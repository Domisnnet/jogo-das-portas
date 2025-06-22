<template>
  <div id="app">
    <h1>Bem-vindo ao Jogo das Portas!!!</h1>

    <!-- Música ambiente com loop -->
    <audio ref="bgMusic" loop>
      <source src="@/assets/sounds/surpni.mp3" type="audio/mpeg" />
      Seu navegador não suporta áudio HTML5.
    </audio>

    <div class="form">
      <div v-if="!started">
        <label for="portsAmount">Número de portas!! </label>
        <input
          type="number"
          id="portsAmount"
          v-model.number="portsAmount"
          min="1"
        />
      </div>

      <div v-if="!started">
        <label for="selectedPort">Qual é a porta premiada? </label>
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

      this.started = true;

      // Define volume e inicia a música ambiente
      const bgMusic = this.$refs.bgMusic;
      bgMusic.volume = 0.01; 
      bgMusic.play().catch((e) => {
        console.warn("Interação do usuário necessária para reproduzir áudio:", e);
      });
    },

    resetGame() {
      this.started = false;
      this.selectedPort = null;

      // Para e reseta a música ambiente
      const bgMusic = this.$refs.bgMusic;
      bgMusic.pause();
      bgMusic.currentTime = 0;
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
  min-height: 100vh;
  color: #fff;
  background: linear-gradient(to top, #240b36, #c31432) no-repeat;
  height: 100vh;
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 100vw;
  padding: 0 10px;
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
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.door-area {
  width: 30vw;
  max-width: 200px;
  min-width: 120px;
  height: auto;
  font-size: 2.5rem;
}

.door {
  width: 100%;
  height: auto;
}

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

@media (max-width: 768px) {
  h1 {
    width: 90%;
    font-size: 1.5rem;
    text-align: center;
  }

  .form {
    width: 90%;
    flex-direction: column;
  }

  input {
    width: 60px;
  }
}
</style>