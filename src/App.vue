<script setup>
import { reactive } from 'vue';

const nome = "fabricio oliveira"
const meuObj = {
  nome: "fabricio",
  filmeFavorito: "Rocky"
}

function dizOla(nome) {
  return `${nome} diz oi`;
}
// Explicação para conseguir colocar uma imagem 
// Coloquei a mesma imagem porque só encontrei imagens base64 com muitas linhas de código
const Batman = "https://upload.wikimedia.org/wikipedia/pt/b/be/Super-Homem.jpg"
const superMan = "https://upload.wikimedia.org/wikipedia/pt/b/be/Super-Homem.jpg"
const botaoEstaDesabilitado = false
const gostaBatman = false
const gostaSuperMan = false


// Criação de um contador no vue.js
const estado = reactive({
  contador:0,
  email: '',
  email1: '',
  saldo: 5000,
  transferindo: 0,
  nomes : ['Ana','Bruno','Carlos','Davi'],
  nomeAInserir: '',
})


function incrementar() {
  estado.contador++;
}

function decrementar() {
  estado.contador--;
}

function alteraEmail (evento) {
  estado.email = evento.target.value;
}

function mostraSaldoFuturo() {
  const { saldo, transferindo } = estado;
  return saldo - transferindo
}

function validaTransferencia() {
  const { saldo, transferindo } = estado;
  return saldo >= transferindo;
}

function cadastraNome() {
  // meu objetivo é não permitir a inserção de informações vazias para a nossa lista!
  if (estado.nomeAInserir.length >= 1) {
    estado.nomes.push(estado.nomeAInserir)
  } else {
    alert("Faz a parada direito, arrombado!")
  }
}

</script>

<template>
  <h1>{{ dizOla("fabricio") }}</h1>
  <!-- O v-bind é utilizado para colocarmos conteudo dinâmico! -->
  <!-- <img v-bind:src="Batman" alt=""> -->
  <!-- O v-if/v-else if/v-else tbm permite a renderização de conteudos dinâmicos -->
  <img v-if="gostaBatman":src="Batman">
  <img v-else-if="gostaSuperMan":src="superMan">
  <h2 v-else>Não gosta de nada</h2>

  <button :disabled="!botaoEstaDesabilitado">Enviar mensagem</button>
  <br>
  <hr>

  {{ estado.contador }}

  <button @click="incrementar" type="button">+</button>
  <button @click="decrementar" type="button">-</button>

  <br>
  <hr>
  {{  estado.email }}
  <br>
  <!-- A função foi encapsulada dentro de alteraEmail, porem pode ser feito com uma arrow function: "evento => estado.email = evento.target.value" -->
  <input type="email" @keyup="alteraEmail">
  <br>
  <!-- O change pode ser usado no local do keyup -->
  <br>
  <hr>
  Saldo: {{ estado.saldo }}<br/>
  Transferindo: {{  estado.transferindo }}<br/>
  Saldo depois da transferência: {{ mostraSaldoFuturo() }}<br/>
  <input :class="{ invalido: !validaTransferencia() }" @keyup="evento => estado.transferindo = evento.target.value" type="number" placeholder="Quantia para transferência">
  <button v-if="validaTransferencia()">Transferir</button>
  <span v-else>Valor maior que o saldo</span>
  <!-- Renderizando listas -->
  <br>
  <hr>
  <ul>
    <li v-for="nome in estado.nomes">
      {{ nome }}
    </li>
  </ul>
  <input @keyup="evento => estado.nomeAInserir = evento.target.value" placeholder="insira um nome">
  <button @click="cadastraNome">Cadastro</button>
</template>

<style scoped>
  input {
      width: 400px;
  }

  .invalido {
    outline-color: red;
    border-color: red;
  }
</style>
