<template>
    <div class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
      <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true"></div>
        <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
        <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
          <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left w-full">
                <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                  Editar Restaurante
                </h3>
                <div class="mt-2">
                  <div>
                    <!-- Formulario de edición de restaurante -->
                    <div class="mb-4">
                      <label for="rating" class="block text-sm font-medium text-gray-700">Id:</label>
                      <input type="number" min="0" max="4" id="rating" v-model="id" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="rating" class="block text-sm font-medium text-gray-700">Rating:</label>
                      <input type="number" min="0" max="4" id="rating" v-model="restaurante.rating" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="name" class="block text-sm font-medium text-gray-700">Nombre:</label>
                      <input type="text" id="name" v-model="restaurante.name" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Sitio web:</label>
                      <input type="text" id="site" v-model="restaurante.site" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Email:</label>
                      <input type="text" id="site" v-model="restaurante.email" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Teléfono:</label>
                      <input type="text" id="site" v-model="restaurante.phone" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Calle:</label>
                      <input type="text" id="site" v-model="restaurante.street" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Ciudad:</label>
                      <input type="text" id="site" v-model="restaurante.city" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Estado:</label>
                      <input type="text" id="site" v-model="restaurante.state" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Latitud:</label>
                      <input type="text" id="site" v-model="restaurante.lat" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                    <div class="mb-4">
                      <label for="site" class="block text-sm font-medium text-gray-700">Longitud:</label>
                      <input type="text" id="site" v-model="restaurante.lng" class="mt-1 p-2 border border-gray-300 rounded w-full">
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
            <button @click="actualizarRestaurante" type="button" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-blue-600 text-base font-medium text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:ml-3 sm:w-auto sm:text-sm">
              Actualizar
            </button>
            <button @click="$emit('cerrar')" type="button" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
              Cancelar
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, getCurrentInstance } from 'vue';
  import axios from 'axios';
  
  const id = ref()
  const restaurante = ref({});
  const { emit } = getCurrentInstance();
  
  const actualizarRestaurante = async () => {
    // Lógica para actualizar el restaurante (enviar el restaurante al backend)
    await axios.put('https://intelimetrica-backend.onrender.com/updateRestaurant/' + id.value, restaurante.value);
    emit('actualizar');
  };
  </script>
  
  <style scoped>
  /* Estilos del modal si los necesitas */
  </style>
  