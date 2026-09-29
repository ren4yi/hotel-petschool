<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); //chamando meu router

//chamando minha api
const API_URL = 'http://localhost3000';

//lista vazia dos meus tutores

const tutores = ref([]);

//criar objeto para salvar um novo pet
//propriedades: nome, espécie, tutorID

const novoPet = ref({
  nome:'',
  especie: '',
  tutorId: ''
});

//buscar todos os tutores que estao salvos na aplicação

async function carregarTutores(){
  const resposta = await fetch(`${API_URL}/tutores`);
  tutores.value = await resposta.json();
}

//converter os dados da minha API que estão em JSON para JS
tutores.value = await resposta.json();

console.log(tutores.value)

//salvar o novo pet no sistema
async function salvarPet() {
await fetch(`${API_URL}/pets`),{
  metthod: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(novoPet.value)
}
}

onMounted(carregarTutores, salvarPet)
</script>

<template>

    <div>

    <header class="mb-4">
        <h1 class="text-2xl">Listagem de Pets</h1>
        <p class = text-body-secondary mb-0>
            Listagem dos Pets cadastrados no sistema
        </p>
    </header>

    <form action="row" @submit.prevent="salvarPet">

      <div class="col-md-6">
        <label for="nome" class="form-label">
          Nome do pet:
        </label>
        <input type="text" class="form-control"
        id="nome"
        v-model="novoPet.nome"
        required>
      </div>

      <div class="colmd-6">
        <label for="especie" class="form-label">Espécie</label>

        <select name="especie" id="especie" class="form-select" required
        v-model="novoPet.especie">
        <option value="" disabled> Selecione a espécie </option>
        <option value="Cachorro">Cachorro</option>
        <option value="Gato">Gato</option>
        <option value="Coelho">Coelho</option>
        <option value="Tartaruga">Tartaruga</option>
        </select>
      </div>

      <div class="col-md-6">
      </div>

      <div class="col-md-6">
      </div>

      <div class="col-md-6">
        <label for="tutor" class="form-label">Tutor</label>
        <select name="tutor" id="tutor" class="form-select" v-model="novoPet.tutorId">
          <option value="" disabled>Selecione o Tutor</option>
          <option v-for="tutor in tutores" :key="tutor.id">{{ tutor.nome }}</option>
        </select>
      </div>

      <div class="col-12 d-flex gap-2">
        <button class="button btn-success" type="submit">
          Salvar Pet
        </button>
      </div>
    </form>

    <RouterLink class=" btn-btn-success" :to="{name: 'novo-pet'}">
        Adicionar Pet
    </RouterLink>

    </div>
    </template>
