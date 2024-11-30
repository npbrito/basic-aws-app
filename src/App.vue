<script setup lang="ts">
import { onMounted, ref } from "vue";
import moment from 'moment'

const filesItems = ref<{ name: string, type: string, date: Date }[]>([])
const file = ref<File | null>(null);
const loadingItems = ref(false);
const loadingUpload = ref(false);

async function getItems() {
  loadingItems.value = true;
  const request = await fetch('http://localhost:3003/v1/files');
  const response = await request.json()
  console.log(response)
  filesItems.value = response;
  loadingItems.value = false;
}

async function uploadFile() {
  if (file.value) {
    loadingUpload.value = true;
    const _body = {
      name: file.value.name,
      type: file.value.type
    }
    await fetch('http://localhost:3003/v1/files', {
      headers: {
        'Content-Type': 'application/json'
      },
      method: 'POST',
      body: JSON.stringify(_body)
    });
   
    const url = await getPresignedUrl()
    await uploadToPresignedUrl(url, file);
   
    file.value = null
    loadingUpload.value = false
  }
}

async function getPresignedUrl() {
  const _body = {

  }
  const request = await fetch('http://localhost:3003/v1/files', {
    headers: {
      'Content-Type': 'application/json'
    },
    method: 'POST',
    body: JSON.stringify(_body)
  });
}

async function uploadToPresignedUrl(url: string, file: File) {
  const request = await fetch('http://localhost:3003/v1/files');
  const response = await request.json()
  console.log(response)
}

async function downloadItem(id: string) {
  const request = await fetch(`http://localhost:3003/v1/files/${id}/download`);
  const response = await request.json()
  console.log(response)
  filesItems.value = response;
}

const handleFileChange = (event: Event) => {
  const input = event.target as HTMLInputElement;
  if (input.files?.length) {
    file.value = input.files[0];
  }
}

onMounted(async () => {
  getItems()
})

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
          <form class="w-full" id="nameForm" @submit.prevent="uploadFile">
            <input v-on:change="handleFileChange" type="file" placeholder="Escolha seu arquivo"
              class="mb-2 mr-4 flex h-full w-full items-center justify-center rounded-lg border border-zinc-800 bg-white/0 px-4 py-4 text-zinc-950 outline-none  md:mb-0" />
          </form>
          <button class="bg-zinc-800 text-white px-6 py-4 rounded-lg " form="nameForm" type="submit">
            <span v-if="!loadingUpload">Enviar</span>
            <svg v-else class="animate-spin h-5 w-5 text-white"
              xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor"
                d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
              </path>
            </svg>
          </button>
        </div>
      </div>

      <div class="flex items-center justify-center">
        <div class="overflow-x-auto sm:rounded-lg border border-zinc-800">
          <table class="w-full text-sm text-left text-gray-500">
            <thead class="text-xs text-gray-700 uppercase border-b border-zinc-800 ">
              <tr>
                <th scope="col" class="py-3 px-6 min-w-[400px]">Nome do arquivo</th>
                <th scope="col" class="py-3 px-6 w-[200px]">Tipo</th>
                <th scope="col" class="py-3 px-6 w-[300px]">Data</th>
                <th scope="col" class="py-3 px-6 w-[80px]"></th>
              </tr>
            </thead>
            <tbody v-if="filesItems.length">
              <tr class="bg-white border-b" v-for="(file, index) in filesItems" :key="index">
                <td class="py-4 px-6">{{ file.name }}</td>
                <td class="py-4 px-6">{{ file.type }}</td>
                <td class="py-4 px-6">{{ moment(file.date).format('DD/MM/YYYY hh:mm:ss') }}</td>                <td class="py-4 px-6">
                  <button>
                    <img src="https://img.icons8.com/?size=100&id=366&format=png" class="w-6 h-6" />
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
          <div v-if="!filesItems.length && !loadingItems" class="text-center py-4">
            <span class="text-sm text-gray-700">Não á registros</span>
          </div>
          <div v-if="!filesItems.length && loadingItems" class="text-center py-4">
            <span class="text-sm text-gray-700">Carregando itens...</span>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>