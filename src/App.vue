

<!-- App.vue -->
<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>Personas</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <formulario-persona @add-persona="agregarPersona" />
        <tabla-personas :personas="personas" @delete-persona="eliminarPersona" @actualizar-persona="actualizarPersona"/>
      </div>
    </div>
  </div>
</template>

<script>
  // Importacion del componente "TablaPersonas"
  import TablaPersonas from '@/components/TablaPersonas.vue'
  import FormularioPersona from '@/components/FormularioPersona.vue'
  import { ref, onMounted } from 'vue';
  // Exportacion del componente principal
  export default {
    // Nombre del componente principal
    name: 'app',
    // Registro del componente "TablaPersonas" para su uso en este componente
    components: {
      TablaPersonas,
      FormularioPersona,
    },
    // Configuracion del componente usando el nuevo sistema "setup"
    setup() {
      const personas = ref([]);

      const agregarPersona = async (persona) => {
        try {
          const response = await fetch('https://my-json-server.typicode.com/rmarabini/people/personas/', {
            method: 'POST',
            body: JSON.stringify(persona),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          });
          const personaCreada = await response.json();
          personas.value = [...personas.value, personaCreada];
        } catch (error) {
          console.error(error);
        }
      };

      const eliminarPersona = async (id) => {
        try {
          await fetch('https://my-json-server.typicode.com/rmarabini/people/personas/'+persona.id+'/', {
            method: "DELETE"
          });
          personas.value= personas.value.filter(u => u.id !== persona.id);
        } catch (error) {
          console.error(error);
        }
      };

      const actualizarPersona = async (id, personaActualizada) => {
        try {
          const response = await fetch('https://my-json-server.typicode.com/rmarabini/people/personas/'+personaActualizada.id+'/', {
            method: 'PUT',
            body: JSON.stringify(personaActualizada),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          });
          const personaActualizadaJS = await response.json();
          personas.value = personas.value.map(u => (u.id === personaActualizada.id ? personaActualizadaJS : u));
        } catch (error) {
          console.error(error);
        }
      };

      const listadoPersonas = async () => {
        try {
          const response = await fetch('https://my-json-server.typicode.com/rmarabini/people/personas/');
          personas.value = await response.json();
        } catch (error) {
          console.error(error);
        }
      };

      onMounted(() => {
        listadoPersonas();
      });

      return {
        personas,
        agregarPersona,
        eliminarPersona,
        actualizarPersona,
      };
    },
  };
</script>

<style scoped>
  /* Estilos globales para todos los elementos button en la aplicacion
  ↪→ */
  button {
    background: #009435;
    border: 1px solid #009435;
  }
</style>

