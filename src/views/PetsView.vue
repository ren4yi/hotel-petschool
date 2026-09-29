<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

//chamar minha api para listar todos os pets

const pets = ref([]); //lista de pets vazia
const tutores = ref([]); // tutores vazio

//chamando a minha api geral
const API_URL = 'http22://localhost:5173'

//chamar a minha API para listar todos os pets

async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();

  console.log('PETS - ', pets.value)
  console.log('TUTORES -', tutores.value)

}

//exibir o nome do tutor

function nomeDoTutor(tutorID){
  for(const tutor of tutores.value){
    if(tutor.id === tutorID){
      return tutor.nome
    }
  }
  return 'Ooops, Tutor não encontrado!'
}

onMounted(nomeDoTutor);
onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'novo-pet' }"
    >
      Adicionar Pet
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Especie</th>
          <th>Tutor</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="pet in pets" :key="pet.id">
          <td>{{ pet.id }}</td>
          <td>{{ pet.nome }}</td>
          <td>{{pet.especie}}</td>
          <td>{{ pet.tutorId }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
