<script setup>
</script>

<template>

    <div>
       
    <header class="mb-4">
        <h1 class="text-2xl">Mostrar Pets</h1>
        <p class = text-body-secondary mb-0>
            Listagem dos Pets cadastrados no sistema
        </p>
    </header>

    <RouterLink class=" btn btn-primary" :to="{name: 'pets'}">
      Listar Pets
    </RouterLink>
     
    </div>
    </template>