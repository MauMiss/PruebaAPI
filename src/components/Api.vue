<template>
  <div class="container mx-auto p-4 w-1/2">
    <div class="bg-blue-400 p-6 rounded-lg">
      <h2 class="text-3xl font-semibold mb-6">Información de la evaluación tutor 2023B</h2>
      <div v-if="isLoading">
        <p class="text-center text-xl font-semibold">Espere un momento, se estan cargando los datos</p>
      </div>
      <div>
        <div class="mb-8">
          <h3 class="text-2xl font-semibold mb-4">Información de la evaluacion de tutor</h3>
          <ul class="list-disc pl-6">
            <li v-for="(value, key) in statusEvalInfo" :key="key" class="mb-2">
              <span class="font-semibold">{{ key }}:</span> {{ displayValue(key, value) }}
            </li>
          </ul></div></div>
        </div>
      </div>
      <div class="container mx-auto p-6 rounded-lg w-1/2 bg-red-800">
        <div >
          <h3 class="text-2xl font-semibold p-6 rounded-lg mb-4 bg-yellow-300">Información de cada Ingeniería</h3>
          <ul class="list-disc pl-6">
            <li v-for="(value, key) in statusEvalIngInfo" :key="key" class="mb-4 text-white">
              <span class="font-semibold">{{ key }}:</span>
              
              <ul class="bg-blue-300 p-6 rounded-lg text-black" >
               
                <li>
                  <span class="font-semibold">Listas:</span> {{ value.listas }}
                </li>
              
                <li>
                  <span class="font-semibold">Faltantes:</span> {{ value.faltantes }}
                </li>
                <li>
                  <span class="font-semibold">Porcentaje completado:</span> {{ calculatePercentage(value) }}%
                </li>
              </ul>
            </li>
          </ul>
        </div></div>
    
     
    
  
</template>

<script setup>
import { ref, onMounted } from 'vue';

const statusEvalInfo = ref({});
const statusEvalIngInfo = ref({});
const isLoading = ref(true);

async function fetchData(url, target) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    console.log(`${target} data:`, data);
    target.value = data;
  } catch (error) {
    console.error(`Error al obtener datos (${target}):`, error);
  }
}

function calculatePercentage(category) {
  // Calcular el porcentaje para la categoría actual
  const totalListas = category.listas || 0;
  const totalFaltantes = category.faltantes || 0;

  return totalListas > 0 ? Math.round((100 * totalFaltantes) / totalListas ) : 0;
}

onMounted(async () => {
  await Promise.all([
    fetchData('https://sitmotul.com.mx/api/statusEval', statusEvalInfo),
    fetchData('https://sitmotul.com.mx/api/statusEvalIng', statusEvalIngInfo),
  ]);
  isLoading.value = false;
});

function formatDate(dateString) {
  // Función formatDate sigue igual, según tus necesidades
 
}

function displayValue(key, value) {
  if (key === 'inicio' || key === 'fin') {
    return formatDate(value);
  } else {
    return value;
  }
}
</script>

<style>
/* Puedes agregar estilos personalizados aquí si es necesario */
</style>
