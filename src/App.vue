<script setup>
import { reactive, toRefs } from 'vue';

const calculadora = reactive({
  numero1: numero1Placeholder,
  numero2: numero2Placeholder,
  operacao: 'null',
  resultado: 0,
  historico: []

});
const { numero1, numero2, operacao, resultado, historico } = toRefs(calculadora);

function calcular() {
  switch (operacao.value) {
    case 'somar':
      resultado.value = numero1.value + numero2.value;
      break;
    case 'subtrair':
      resultado.value = numero1.value - numero2.value;
      break;
    case 'multiplicar':
      resultado.value = numero1.value * numero2.value;
      break;
    case 'dividir':
      resultado.value = numero1.value / numero2.value;
      break;
  }

  const operacaoTexto = `${numero1.value} ${operacao.value} ${numero2.value} = ${resultado.value}`;
  historico.value.unshift(operacaoTexto);

  if (historico.value.length > 5){
    historico.value.pop()
  }
}

function numero1Hist(item) {
  return item.split(' ')[0];
}
function operacaoHist(item) {
  return item.split(' ')[1];
}
function numero2Hist(item) {
  return item.split(' ')[2];
}
function resultadoHist(item) {
  return item.split(' ')[4];
}

const numero1Placeholder = 'Digite um número';
const numero2Placeholder = 'Digite um número';
</script>

<template>
  <div class="calculadora-container">
    <header class="header">
    <h1>Calculadora Vue.JS</h1>
    <p>João Miguel Custódio</p>
    </header>
    <form class="calculadora-form">
        <div class="form-group">
            <div class="input-container">
                <input v-model="numero1" type="number" :placeholder="numero1Placeholder">
                <input v-model="numero2" type="number" :placeholder="numero2Placeholder">
            </div>
        </div>
        <div class="form-group">
            <select v-model="operacao" class="operacao-select" @change="calcular">
                <option value="null" disable selected>Selecione a operação desejada</option>
                <option value="somar">Somar</option>
                <option value="subtrair">Subtrair</option>
                <option value="multiplicar">Multiplicar</option>
                <option value="dividir">Dividir</option>
            </select>
        </div>
    </form>
    <div class="resultados">
        <div class="resultado-header">
            <div class="col">Numero 1</div>
            <div class="col">Operação</div>
            <div class="col">Numero 2</div>
            <div class="col">Resultados</div>
        </div>
    </div>
    <ul class="list-group">
        <li class="list-group-item" v-for="(item, index) in historico" :key="index" :class="{ destaque: index === 0 }">
            <div class="resultado-row">
                <div class="col"> {{ numero1Hist(item) }}</div>
                <div class="col"> {{ operacaoHist(item) }}</div>
                <div class="col"> {{ numero2Hist(item) }}</div>
                <div class="col"> {{ resultadoHist(item) }}</div>
            </div>
        </li>
    </ul>
  </div>
</template>

<style scoped>
.calculadora-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 10px;
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.calculadora-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.input-container {
  display: flex;
  gap: 10px;
}

.operacao-select {
  width: 100%;
  padding: 5px;
  border-radius: 5px;
}
.operacao-label {
  margin-top: 10px;
}

.resultados {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
}

.resultado-header {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  font-weight: bold;
  border-bottom: 1px solid #ddd;
  padding-bottom: 5px;
  margin-bottom: 10px;
}

.list-group-item {
  border: none;
  padding: 5px;
}

.destaque {
  background-color: #f5f5f5;
  font-weight: bold;
}

.resultado-row {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
</style>
