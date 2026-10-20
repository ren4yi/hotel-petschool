
<script setup>
import { onMounted, ref } from 'vue';
import { useRoute, RouterLink } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';

const tutor = ref(null);
const pets = ref([]);

const novoPet = ref({
    nome: '',
    especie: '',
    tutorId: '',
});

// buscar os dados do tutor e seus pets
async function carregarPet() {
    const idTutor = route.params.id;

    const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
    tutor.value = await respostaTutor.json();

    const dataPets = await fetch(`${API_URL}/pets`);
    pets.value = await dataPets.json();
}

// buscar somente os pets vinculados ao tutor
function getPets(tutorId) {
    const petsDesteTutor = [];

    for (const pet of pets.value) {
        if (String(pet.tutorId) === String(tutorId)) {
            petsDesteTutor.push(pet);
        }
    }

    return petsDesteTutor;
}

// cadastrar um novo pet vinculado ao tutor
async function salvarPet() {
    novoPet.value.tutorId = tutor.value.id;

    const resposta = await fetch(`${API_URL}/pets`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(novoPet.value),
    });

    if (!resposta.ok) {
        alert('Erro ao cadastrar pet!');
        return;
    }

    // limpar o formulário
    novoPet.value = {
        nome: '',
        especie: '',
        tutorId: '',
    };

    // atualizar a lista de pets
    await carregarPet();

    alert('Pet cadastrado com sucesso!');
}

onMounted(carregarPet);
</script>

<template>
    <section v-if="tutor == null">
        <p>Carregando...</p>
    </section>

    <section v-else class="container-fluid">
        <div class="d-flex justify-content-between align-items-start mb-3">
            <div>
                <h2 class="fw-bold mb-0">Detalhes do Tutor</h2>
                <p class="text-secondary mb-0">
                    Visualize os dados do tutor e cadastre pets vinculados.
                </p>
            </div>

            <RouterLink
                to="/tutores"
                class="btn btn-outline-primary"
            >
                ← Voltar para Tutores
            </RouterLink>
        </div>

        <!-- informações do tutor -->
        <div class="card mb-3">
            <div class="card-header">
                <h5 class="mb-0">Informações do Tutor</h5>
            </div>

            <div class="card-body">
                <div class="card mb-2">
                    <div class="card-header">
                        <h6 class="mb-0">Dados do Tutor</h6>
                    </div>

                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">ID</label>
                                <p class="mb-1">{{ tutor.id }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Nome Completo</label>
                                <p class="mb-1">{{ tutor.nome }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">CPF / CNPJ</label>
                                <p class="mb-1">{{ tutor.cpf }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Telefone / Celular / WhatsApp</label>
                                <p class="mb-1">{{ tutor.telefone }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">E-mail</label>
                                <p class="mb-1">{{ tutor.email }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Data de Nascimento</label>
                                <p class="mb-1">{{ tutor.dataNascimento || 'Não informado' }}</p>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div class="card-header">
                        <h6 class="mb-0">Endereço</h6>
                    </div>

                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Rua</label>
                                <p class="mb-1">{{ tutor.rua }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Número</label>
                                <p class="mb-1">{{ tutor.numerocasa || tutor.numero }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Bairro</label>
                                <p class="mb-1">{{ tutor.bairro }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Cidade</label>
                                <p class="mb-1">{{ tutor.cidade }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">Estado</label>
                                <p class="mb-1">{{ tutor.estado }}</p>
                            </div>

                            <div class="col-md-4 mb-2">
                                <label class="fw-bold">CEP</label>
                                <p class="mb-1">{{ tutor.cep }}</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- pets do tutor -->
        <div class="card mb-3">
            <div class="card-header">
                <h5 class="mb-0">Pets do Tutor</h5>
            </div>

            <div class="card-body">
                <div class="row g-3">
                    <!-- tabela de pets -->
                    <div class="col-lg-6">
                        <div class="card h-100">
                            <div class="card-header">
                                <h6 class="mb-0">
                                    Pets vinculados
                                    <span class="badge rounded-pill text-bg-primary">
                                        {{ getPets(tutor.id).length }} pets
                                    </span>
                                </h6>
                            </div>

                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-striped table-hover">
                                        <thead>
                                            <tr>
                                                <th>ID</th>
                                                <th>Nome</th>
                                                <th>Espécie</th>
                                                <th>Ações</th>
                                            </tr>
                                        </thead>

                                        <tbody>
                                            <tr
                                                v-for="pet in getPets(tutor.id)"
                                                :key="pet.id"
                                            >
                                                <td>{{ pet.id }}</td>
                                                <td>{{ pet.nome }}</td>
                                                <td>{{ pet.especie }}</td>
                                                <td>
                                                    <RouterLink
                                                        :to="`/pets/${pet.id}`"
                                                        class="btn btn-sm btn-outline-primary"
                                                    >
                                                        Visualizar
                                                    </RouterLink>
                                                </td>
                                            </tr>

                                            <tr v-if="getPets(tutor.id).length === 0">
                                                <td colspan="4" class="text-center">
                                                    Nenhum pet cadastrado.
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- formulário de cadastro de pet -->
                    <div class="col-lg-6">
                        <div class="card h-100">
                            <div class="card-header">
                                <h6 class="mb-0">Cadastrar novo pet</h6>
                                <small class="text-secondary">
                                    O pet será vinculado automaticamente a {{ tutor.nome }}.
                                </small>
                            </div>

                            <div class="card-body">
                                <form @submit.prevent="salvarPet">
                                    <div class="mb-3">
                                        <label for="nomePet" class="form-label fw-bold">
                                            Nome do pet
                                        </label>

                                        <input
                                            type="text"
                                            class="form-control"
                                            id="nomePet"
                                            v-model="novoPet.nome"
                                            placeholder="Digite o nome do pet"
                                            required
                                        />
                                    </div>

                                    <div class="mb-3">
                                        <label for="especiePet" class="form-label fw-bold">
                                            Espécie
                                        </label>

                                        <select
                                            class="form-select"
                                            id="especiePet"
                                            v-model="novoPet.especie"
                                            required
                                        >
                                            <option value="" disabled>
                                                Selecione
                                            </option>
                                            <option value="Gato">Gato</option>
                                            <option value="Cachorro">Cachorro</option>
                                            <option value="Ave">Ave</option>
                                            <option value="Outro">Outro</option>
                                        </select>
                                    </div>

                                    <button
                                        type="submit"
                                        class="btn btn-success w-100"
                                    >
                                        Cadastrar pet
                                    </button>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>
