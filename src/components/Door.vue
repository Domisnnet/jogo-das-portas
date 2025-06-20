<template>
    <div class="door-area">
      <div class="door-frame" :class="{ selected }">
        <Gift v-if="open && hasGift" />
      </div>
      <div class="door" :class=" { open } " 
      @click="toggleDoor">
        <div class="number" :class="{ selected }">{{ number }}</div>
        <div class="knob" :class="{ selected }"
        @click="open = true"></div>
      </div>
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
        this.open = !this.open;
        this.selected = !this.selected;
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
  }

  .door.open .knob {
    display: none;
  }

  .door.open .number {
    display: none;
  }
  </style>  