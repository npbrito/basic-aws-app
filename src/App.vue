<script setup lang="ts">
import { ref } from "vue";
import moment from 'moment'

const tableData = ref([])

// TODO: add functions

</script>

<template>
  <main class="flex justify-center">
    <div class="max-w-[1400px] mt-8">
      <div
        class="rounded-lg border bg-card text-card-foreground shadow-sm mb-6 h-min max-w-full pt-8 pb-6 px-6 border-zinc-800">
        <p class="text-xl font-bold text-zinc-950  md:text-3xl">
          Teste de bucket
        </p>
        <p class="mb-6 mt-1 text-sm font-medium text-zinc-500  md:mt-4 md:text-base">
          Escolha um arquivo para subir para o bucket privado
        </p>
        <div class="mb-8 flex flex-col md:flex-row gap-4">
          <form class="w-full" id="nameForm">
            <input type="file" placeholder="Escolha seu arquivo"
              class="mb-2 mr-4 flex h-full w-full items-center justify-center rounded-lg border border-zinc-800 bg-white/0 px-4 py-4 text-zinc-950 outline-none  md:mb-0"
              />
          </form>
          <button
                class="bg-zinc-800 text-white px-6 py-4 rounded-lg "
                form="nameForm"
                type="submit"
              >
                Enviar
              </button>
        </div>
      </div>

      <div class="flex items-center justify-center">
        <div class="overflow-x-auto sm:rounded-lg border border-zinc-800">
          <table class="w-full text-sm text-left text-gray-500 min-w-[80vw]">
            <thead class="text-xs text-gray-700 uppercase border-b border-zinc-800 ">
              <tr>
                <th scope="col" class="py-3 px-6">Nome do arquivo</th>
                <th scope="col" class="py-3 px-6 w-[200px]">Data</th>
                <th scope="col" class="py-3 px-6 w-[80px]"></th>
              </tr>
            </thead>
            <tbody v-if="tableData.length">
              <tr class="bg-white border-b" v-for="(data, index) in tableData" :key="index">
                <td class="py-4 px-6">{{ data.name }}</td>
                <td class="py-4 px-6">{{ moment(data.date).format('DD/MM/YYYY hh:mm:ss') }}</td>
                <td class="py-4 px-6">
                  <button>
                    <img src="https://img.icons8.com/?size=100&id=366&format=png" class="w-6 h-6"/>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
          <div v-if="!tableData.length" class="text-center py-4" >
            <span class="text-sm text-gray-700">Não á registros</span>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>