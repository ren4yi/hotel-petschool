<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); // chamando meu router
// chamando a minha API
const API_URL = 'http://localhost:3000';

// lista vazia dos meus tutores
const tutores = ref([]);

// criar objeto para salvar um novo pet
// propriedades: nome, especie e tutorId

const novoTutor = ref({
  nome: '',
  telefone: '',
  cpf: '',
  email: '',
  rua: '',
  numero: '',
  bairro: '',
  estado: '',
  cidade: '',
  cep: '',
  numerocasa: '',
});

// buscar todos os tutores que estão salvo na aplicação
async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);

  // converter os dados da minha API que estão em JSON para JS
  tutores.value = await resposta.json();
  console.table(tutores.value);
}

// salvar o novo tutor no sistema
async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });

  // redirecionar para a tela de listagem de Pets
  router.push('/tutores');
}

onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastrar novos tutores</h1>
      <p class="text-body-secondary mb-0">Cadastro de tutores no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-outline-primary"
      :to="{ name: 'tutores' }"
    >
      Lista tutores
    </RouterLink>

    <form
      class="row"
      @submit.prevent="salvarTutor"
    >
      <!-- nome do pet -->
      <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
        >
          Nome completo:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoTutor.nome"
          required
        />
      </div>

      <div class="col-md-6">
        <label
          for="cpf"
          class="form-label"
        >
         CPF / CNPJ
        </label>
        <input
          type="text"
          class="form-control"
          id="cpf"
          v-model="novoTutor.cpf"
          required
        />
      </div>

       <div class="col-md-6">
        <label
          for="telefone"
          class="form-label"
        >
        Telefone / Celular / Whatsapp
        </label>
        <input
          type="text"
          class="form-control"
          id="telefone"
          v-model="novoTutor.telefone"
          required
        />
      </div>

       <div class="col-md-6">
        <label
          for="email"
          class="form-label"
        >
         E-mail
        </label>
        <input
          type="text"
          class="form-control"
          id="email"
          v-model="novoTutor.email"
          required
        />
      </div>

      <!-- especie do pet -->
      <div class="col-md-6">
        <label
          class="form-label"
          for="estado"
          >Estado</label
        >
        <select
          name="estado"
          id="estado"
          class="form-select"
          required
          v-model="novoTutor.estado"
        >
          <option
            value=""
            disabled
          >
            Selecione o estado:
          </option>
          <option value="SC">Santa Catarina</option>
          <option value="MG">Minas Gerais</option>
          <option value="RS">Rio Grande do Sul</option>
          <option value="SP">São Paulo</option>
        </select>
      </div>

       <div class="col-md-6">
        <label
          for="cidade"
          class="form-label"
        >
         Cidade
        </label>
        <input
          type="text"
          class="form-control"
          id="cidade"
          v-model="novoTutor.cidade"
          required
        />
      </div>

<div class="col-md-6">
        <label
          for="bairro"
          class="form-label"
        >
         Bairro
        </label>
        <input
          type="text"
          class="form-control"
          id="bairro"
          v-model="novoTutor.bairro"
          required
        />
      </div>

    <div class="col-md-6">
        <label
          for="rua"
          class="form-label"
        >
         Rua
        </label>
        <input
          type="text"
          class="form-control"
          id="rua"
          v-model="novoTutor.rua"
          required
        />
      </div>

        <div class="col-md-6">
        <label
          for="numerocasa"
          class="form-label"
        >
         Número
        </label>
        <input
          type="text"
          class="form-control"
          id="numerocasa"
          v-model="novoTutor.numerocasa"
          required
        />
      </div>

      <div class="col-md-6">
        <label
          for="cep"
          class="form-label"
        >
         CEP
        </label>
        <input
          type="text"
          class="form-control"
          id="cep"
          v-model="novoTutor.cep"
          required
        />
      </div>

      <div class="col-12 d-flex gap-2">
        <button
          class="btn btn-success"
          type="submit"
        >
          Salvar tutor
        </button>
      </div>
    </form>
  </div>
</template>
