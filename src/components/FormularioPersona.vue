<!-- FormularioPersona.vue -->
<template>
    <!-- Contenedor principal del componente -->
    <div id="formulario-persona">
        <!-- Formulario con campos para ingresar informacion de una persona -->
        <form @submit.prevent="enviarFormulario">
            <div class="container">
            <!-- Primera fila con campos de nombre, apellido y email -->
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <!-- Etiqueta y campo de entrada para el nombre -->
                            <label>Nombre</label>
                            <input v-model="persona.nombre" type="text" class="form-control" data-cy="name"
                            :class="{ 'is-invalid': procesando && nombreInvalido }"
                            @focus="resetEstado"
                            @keypress="resetEstado"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <!-- Etiqueta y campo de entrada para el apellido -->
                            <label>Apellido</label>
                            <input v-model="persona.apellido" type="text" class="form-control" data-cy="surname"
                            :class="{ 'is-invalid': procesando && apellidoInvalido }"
                            @focus="resetEstado"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <!-- Etiqueta y campo de entrada para el correo electronico -->
                            <label>Email</label>
                            <input v-model="persona.email" type="text" class="form-control" data-cy="surname"
                            :class="{ 'is-invalid': procesando && apellidoInvalido }"
                            @focus="resetEstado"
                            />
                        </div>
                    </div>
                </div>
                <!-- Segunda fila con un boton para agregar persona -->
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <!-- Boton para agnadir persona -->
                            <button class="btn btn-primary" data-cy="add-button">Anyadir persona</button>
                        </div>
                    </div>
                </div>
            </div>
            <br />
            <div class="container">
                <div class="row">
                    <div class="col-md-12">
                        <div v-if="error && procesando" class="alert alert-danger" role="alert">
                            Debes rellenar todos los campos!
                        </div>
                        <div v-if="correcto" class="alert alert-success" role="alert">
                            La persona ha sido agregada correctamente!
                        </div>
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>
<script>
    // Importacion de la funcion "ref" de Vue 3
    import { ref, defineEmits, computed } from 'vue';
    // Exportacion del componente "FormularioPersona"
    export default {
        // Nombre del componente
        name: 'formulario-persona',
        // Configuracion del componente usando el sistema "setup"
        setup(props, ctx) {
            const procesando = ref(false);
            const correcto = ref(false);
            const error = ref(false);

            const nombre = ref(null);

            // Define emisiones personalizadas para este componente
            const emit = defineEmits();

            const nombreInvalido = computed(() => persona.value.nombre.length < 1);
            const apellidoInvalido = computed(() => persona.value.apellido.length < 1);
            const emailInvalido = computed(() => persona.value.email.length < 1);

            // Declaracion de una variable reactiva "persona" con propiedades nombre, apellido y email
            const persona = ref({
                nombre: '',
                apellido: '',
                email: '',
            });

            const enviarFormulario = () => {
                procesando.value = true;
                resetEstado();
                
                if (nombreInvalido.value || apellidoInvalido.value || emailInvalido.value) {
                    error.value = true;
                    return;
                }

                ctx.emit('add-persona', persona.value);
                //nombre.value.focus();

                persona.value = {
                    nombre: '',
                    apellido: '',
                    email: '',
                };
                error.value = false;
                correcto.value = true;
                procesando.value = false;
            };

            const resetEstado = () => {
                correcto.value = false;
                error.value = false;
            };

            // Retorno de las variables o funciones que el componente va a exponer
            return {
                persona,
                nombre,
                error,
                correcto,
                procesando,
                resetEstado,
                nombreInvalido,
                apellidoInvalido,
                emailInvalido,
                enviarFormulario,
            };
        },
    };
</script>
<style scoped>
    /* Estilos especificos del componente con el modificador "scoped" */
    form {
        margin-bottom: 2rem;
    }
</style>