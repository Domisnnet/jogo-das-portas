<template>
  <div class="game-wrapper">
    <div class="door-area">
      <div
        class="door"
        :class="{ open, error: errorState }"
        :style="doorStyle"
        @click="toggleDoor"
      />
      <Gift v-if="open && hasGift" class="gift" />
    </div>
    <div class="explosion" ref="explosion" />
  </div>
</template>

<script>
import Gift from './Gift.vue';

export default {
  name: 'DoorComponent',
  components: { Gift },
  props: {
    number: { type: Number, required: true },
    hasGift: { type: Boolean, default: false }
  },
  data() {
    return {
      open: false,
      errorState: false
    };
  },
  computed: {
    doorStyle() {
      const image = this.open
        ? require('@/assets/img/door-open.png')
        : require('@/assets/img/door.png');

      return {
        backgroundImage: `url(${image})`
      };
    }
  },
  methods: {
    toggleDoor() {
      if (this.open) return;
      this.open = true;

      if (this.hasGift) {
        setTimeout(() => this.playGiftSound(), 400);
      } else {
        this.playErrorExplosion();
      }
    },
    playGiftSound() {
      const audio = new Audio(require('@/assets/sounds/wow.mp3'));
      audio.volume = 0.7;
      audio.play();
    },
    playErrorExplosion() {
      const audio = new Audio(require('@/assets/sounds/explosion.mp3'));
      audio.volume = 0.2;
      audio.play();

      this.errorState = true;
      setTimeout(() => (this.errorState = false), 500);

      const boom = this.$refs.explosion;
      boom?.classList.add('mega-blast');
      setTimeout(() => boom?.classList.remove('mega-blast'), 600);
    }
  }
};
</script>

<style scoped>
.game-wrapper {
  position: relative;
  width: 100%;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: visible;
}

.door-area {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  overflow: visible;
}

.door {
  width: 170px;
  aspect-ratio: 655 / 1318;
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  transition: background-image 0.4s ease;
  cursor: pointer;
  position: relative;
  z-index: 2;
  overflow: visible;
}

.door.error {
  animation: shake 0.4s ease;
}

@keyframes shake {
  0% { transform: translateX(0); }
  25% { transform: translateX(-6px); }
  50% { transform: translateX(6px); }
  75% { transform: translateX(-4px); }
  100% { transform: translateX(0); }
}

.gift {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 3;
  animation: aparecer 0.4s ease forwards;
}

@keyframes aparecer {
  0% { opacity: 0; transform: translate(-50%, -50%) scale(0.6); }
  100% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
}

.explosion {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.3);
  width: 120px;
  height: 120px;
  background: radial-gradient(circle, #fff, #ff0, red, transparent);
  border-radius: 50%;
  opacity: 0;
  pointer-events: none;
  filter: blur(0px);
  z-index: 10;
}

.explosion.mega-blast {
  animation: big-boom 0.6s ease-out forwards;
}

@keyframes big-boom {
  0% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(0.3);
    box-shadow: 0 0 20px 10px #ff0;
    filter: blur(0px);
  }
  40% {
    transform: translate(-50%, -50%) scale(2.5);
    box-shadow: 0 0 80px 40px red;
    filter: blur(3px);
  }
  80% {
    opacity: 0.8;
    transform: translate(-50%, -50%) scale(3.2);
    box-shadow: 0 0 120px 60px orange;
    filter: blur(8px);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.1);
    box-shadow: none;
    filter: blur(0px);
  }
}
</style>