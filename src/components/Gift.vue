<template>
    <transition name="fade">
      <div v-if="show" class="gift-wrapper">
        <img
          src="../assets/img/gift.png" alt="Presente" width="200" height="200"
        />
      </div>
    </transition>
  </template>
  
  <script>
  export default {
    name: "GiftComponent",
    props: {
      delay: {
        type: Number,
        default: 600 
      }
    },
    data() {
      return {
        show: false
      };
    },
    mounted() {
      setTimeout(() => {
        this.show = true;
      }, this.delay);
    }
  };
  </script>
  
  <style scoped>
  .gift-wrapper {
    position: absolute;
    bottom: -20px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 5;
    pointer-events: none;
    max-height: 200px;
    animation: aparecer 0.5s ease;
    overflow: hidden;
  }
  
  .gift-wrapper img {
    margin-top: -15px; 
  }
  
  @keyframes aparecer {
    0% {
      opacity: 0;
      transform: translateX(-50%) scale(0.6);
    }
    100% {
      opacity: 1;
      transform: translateX(-50%) scale(1);
    }
  }
  
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
  </style>