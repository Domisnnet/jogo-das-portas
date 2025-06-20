<template>
  <div id="app">
    <h1>Bem Vindo ao Jogo das Portas!!!</h1>
    <Gift />
  </div>
</template>

<script>
  import Gift from "./components/Gift.vue";

export default {
  name: "App",
  components: { Gift },
}
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
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#app h1 {
  width: 580px;
  border: 1px solid #000;
  background-color: #0004;
  padding: 20px;
  margin-bottom: 60px;
  border-radius: 15px;
}

#app p {
  font-size: 4rem;
  text-align: center;
  margin: 0; 
}
</style>
