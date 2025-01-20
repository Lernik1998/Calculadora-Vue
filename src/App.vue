<script setup>
import Calculadora from "./components/Calculadora.vue";
</script>

<template>
  <header>
    <h1>Calculadora con VUE</h1>
    <h2>Creado por Lernik </h2>

    <h3><a href="https://spiffy-cassata-dc43e3.netlify.app/">Proyecto Desplegado</a></h3>
  </header>

  <main>
    <!-- Aqui incluimos el componente de la CALCULADORA -->
    <Calculadora />
  </main>
</template>

<style scoped>
  h1,h2{
    color: blue;
    text-align: center;
  }
</style>

