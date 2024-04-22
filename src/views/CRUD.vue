<template>
  <div class="container mx-auto p-4">
    <!-- Sección para agregar nuevo restaurante -->
    <div class="mb-4">
      <h2 class="text-lg font-semibold mb-2">Agregar Nuevo Restaurante</h2>
      <!-- Formulario para agregar un nuevo restaurante -->
      <form @submit.prevent="agregarRestaurante">
        <input v-model="nuevoRestaurante.id" type="text" placeholder="ID" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.rating" type="number" min="0" max="5" placeholder="Rating (0-4)" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.name" type="text" placeholder="Nombre" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.site" type="text" placeholder="Sitio web" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.email" type="email" placeholder="Correo electrónico" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.phone" type="text" placeholder="Teléfono" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.street" type="text" placeholder="Calle" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.city" type="text" placeholder="Ciudad" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.state" type="text" placeholder="Estado" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.lat" type="number" step="any" placeholder="Latitud" class="mr-2 p-2 border border-gray-300 rounded">
        <input v-model="nuevoRestaurante.lng" type="number" step="any" placeholder="Longitud" class="mr-2 p-2 border border-gray-300 rounded">
        <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Agregar</button>
      </form>
    </div>

    <!-- Contenedor para centrar la tabla -->
    <div class="flex justify-center">
      <!-- Tabla para mostrar los restaurantes -->
      <div>
        <h2 class="text-lg font-semibold mb-2">Lista de Restaurantes</h2>
        <table class="w-full">
          <thead>
            <tr>
              <th class="px-4 py-2">Rating</th>
              <th class="px-4 py-2">Nombre</th>
              <th class="px-4 py-2">Sitio web</th>
              <th class="px-4 py-2">Correo electrónico</th>
              <th class="px-4 py-2">Teléfono</th>
              <th class="px-4 py-2">Calle</th>
              <th class="px-4 py-2">Ciudad</th>
              <th class="px-4 py-2">Estado</th>
              <th class="px-4 py-2">Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(restaurante, index) in restaurantes" :key="index">
              <td class="border px-4 py-2">{{ restaurante.rating }}</td>
              <td class="border px-4 py-2">{{ restaurante.name }}</td>
              <td class="border px-4 py-2">{{ restaurante.site }}</td>
              <td class="border px-4 py-2">{{ restaurante.email }}</td>
              <td class="border px-4 py-2">{{ restaurante.phone }}</td>
              <td class="border px-4 py-2">{{ restaurante.street }}</td>
              <td class="border px-4 py-2">{{ restaurante.city }}</td>
              <td class="border px-4 py-2">{{ restaurante.state }}</td>
              <td class="border px-4 py-2">
                <button @click="abrirModalEditar(restaurante)" class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-1 px-2 rounded mr-2">Editar</button>
                <button @click="eliminarRestaurante(restaurante.id)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-1 px-2 rounded">Eliminar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- Modal para editar restaurante -->
    <EditarRestauranteModal @actualizar="cerrarModalEditar" @cerrar="cerrarModalEditar" v-if="mostrarModalEditar" />
    <EliminarRestauranteModal :idRegistro="idRegistroAEliminar" @eliminar="cerrarModalBorrar" @cerrar="cerrarModalBorrar  " v-if="mostrarModalBorrar" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import EditarRestauranteModal from '../components/actualizarRestaurante.vue';
import EliminarRestauranteModal from '../components/eliminarRestaurante.vue'

// Datos reactivos
const restaurantes = ref([]);
const nuevoRestaurante = ref({ 
  id: '',
  rating: 0,
  name: '',
  site: '',
  email: '',
  phone: '',
  street: '',
  city: '',
  state: '',
  lat: 0,
  lng: 0
});
const restauranteEditando = ref(null); // Restaurante actualmente editando
const mostrarModalEditar = ref(false);
const mostrarModalBorrar = ref(false);
const idRegistroAEliminar = ref();

const abrirModalEditar = (restaurante) => {
  // Clonar el restaurante para evitar mutaciones no deseadas
  restauranteEditando.value = { ...restaurante };
  mostrarModalEditar.value = true;
};

const cerrarModalEditar = () => {
  mostrarModalEditar.value = false;
};

const abrirModalBorrar = (id) => {
  idRegistroAEliminar.value = id
  mostrarModalBorrar.value = true
}

const cerrarModalBorrar = () => {
  mostrarModalBorrar.value = false
}

// Métodos
async function agregarRestaurante() {
  // Lógica para agregar un nuevo restaurante (usando Axios para hacer la petición POST)
  try {
    const response = await axios.post('https://intelimetrica-backend.onrender.com/createRestaurant', nuevoRestaurante.value);
    restaurantes.value.push(response.data);
    nuevoRestaurante.value = { 
      id: '',
      rating: 0,
      name: '',
      site: '',
      email: '',
      phone: '',
      street: '',
      city: '',
      state: '',
      lat: 0,
      lng: 0
    }; // Limpiar el formulario después de agregar
  } catch (error) {
    console.error('Error al agregar el restaurante:', error);
  }
}

function editarRestaurante(index) {
  // Lógica para editar un restaurante
  const restauranteAEditar = restaurantes.value[index];
  // Lógica para editar el restaurante...
}

async function eliminarRestaurante(id) {
  console.log(id)
  await axios.delete('https://intelimetrica-backend.onrender.com/deleteRestaurant/' + id);
  console.log('Usuario borrado');
}

// Lógica para cargar los restaurantes al cargar la página (usando Axios para hacer la petición GET)
import { onMounted } from 'vue';
onMounted(async () => {
  try {
    const response = await axios.get('https://intelimetrica-backend.onrender.com/getRestaurants');
    restaurantes.value = response.data;
  } catch (error) {
    console.error('Error al cargar los restaurantes:', error);
  }
});

</script>

<style>

</style>
