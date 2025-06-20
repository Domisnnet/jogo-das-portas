<template>
  <div class="door-area">
    <div class="door-frame" :class="{ selected }">
      <Gift v-if="open && hasGift" />
    </div>
    <div class="door" :class="{ open }" @click="toggleDoor">
      <div class="number" :class="{ selected }">{{ number }}</div>
      <div class="knob" :class="{ selected }"></div>
    </div>
    <!-- Explosão -->
    <div class="explosion" ref="explosion" />
  </div>
</template>

<script>
import Gift from "./Gift.vue";

export default {
  name: "DoorComponent",
  components: { Gift },
  props: {
    number: {
      type: Number,
      required: true
    },
    hasGift: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      open: false,
      selected: false
    };
  },
  methods: {
    toggleDoor() {
      if (this.open) return;

      this.open = true;
      this.selected = true;

      if (this.hasGift) {
        setTimeout(() => this.playGiftSound(), 400);
      } else {
        this.playErrorExplosion();
      }
    },
    playGiftSound() {
      const audio = new Audio(require('@/assets/sounds/wow.mp3'));
      audio.volume = 0.8;
      audio.play();
    },
    playErrorExplosion() {
      // Som de explosão mais forte
      const audio = new Audio(require('@/assets/sounds/explosion.mp3'));
      audio.volume = 1.0;
      audio.play();

      // Animação de tremor
      const doorEl = this.$el.querySelector('.door');
      if (doorEl) {
        doorEl.classList.add('error');
        setTimeout(() => doorEl.classList.remove('error'), 500);
      }

      // Animação visual de explosão
      const explosionEl = this.$refs.explosion;
      if (explosionEl) {
        explosionEl.classList.add('mega-blast');
        setTimeout(() => explosionEl.classList.remove('mega-blast'), 600);
      }
    }
  }
};
</script>

<style>
:root {
  --door-border: 5px solid brown;
  --selected-border: 5px solid #ff0;
}

.door-area {
  position: relative;
  display: flex;
  justify-content: center;
  width: 200px;
  height: 310px;
  border-bottom: 10px solid #aaa;
  margin-bottom: 20px;
  font-size: 3rem;
}

.door-frame {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  width: 180px;
  height: 300px;
  border-left: var(--door-border);
  border-top: var(--door-border);
  border-right: var(--door-border);
}

.door-frame.selected {
  border-left: var(--selected-border);
  border-top: var(--selected-border);
  border-right: var(--selected-border);
}

.door {
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  top: 5px;
  height: 295px;
  width: 170px;
  background-color: chocolate;
  cursor: pointer;
  transition: background-color 0.3s;
}

.door.error {
  animation: shake 0.4s ease;
  box-shadow: 0 0 20px 6px red;
}

@keyframes shake {
  0% { transform: translateX(0); }
  25% { transform: translateX(-6px); }
  50% { transform: translateX(6px); }
  75% { transform: translateX(-4px); }
  100% { transform: translateX(0); }
}

.number {
  font-weight: bold;
  padding: 10px;
  border-radius: 10px;
}

.number.selected {
  background-color: #ff0;
  color: #000;
}

.knob {
  width: 20px;
  height: 20px;
  background-color: brown;
  border-radius: 50%;
  margin-top: 60px;
  margin-left: 100px;
}

.knob.selected {
  background-color: #ff0;
}

.door.open {
  background-color: #0007;
  box-shadow: 0 0 20px 5px #ffd700aa;
}

.door.open .knob,
.door.open .number {
  display: none;
}

/* Explosão maior */
.explosion {
  position: absolute;
  top: 25%;
  left: 50%;
  transform: translateX(-50%) scale(0.3);
  width: 120px;
  height: 120px;
  pointer-events: none;
  background: radial-gradient(circle, #fff, #ff0, red, transparent);
  border-radius: 50%;
  opacity: 0;
  z-index: 10;
  filter: blur(0px);
}

.explosion.mega-blast {
  animation: big-boom 0.6s ease-out forwards;
}

@keyframes big-boom {
  0% {
    opacity: 1;
    transform: translateX(-50%) scale(0.3);
    box-shadow: 0 0 20px 10px #ff0;
    filter: blur(0px);
  }
  40% {
    transform: translateX(-50%) scale(2.5);
    box-shadow: 0 0 80px 40px red;
    filter: blur(3px);
  }
  80% {
    opacity: 0.8;
    transform: translateX(-50%) scale(3.2);
    box-shadow: 0 0 120px 60px orange;
    filter: blur(8px);
  }
  100% {
    opacity: 0;
    transform: translateX(-50%) scale(0.1);
    box-shadow: none;
    filter: blur(0px);
  }
}
</style>