<template>
  <div id="app">
    <div class="container">
      <nav style="text-align: center" class="navbar navbar-expand-lg navbar-light bg-light">
        <span class="nav-item">
          <a class="nav-link" href="#">Estudiantes</a>
        </span>
        |
        <span class="nav-item">
          <a class="nav-link" href="#">Cursos</a>
        </span>
      </nav>
      <mi-alerta
        v-if="alerta"
        @ok="alerta = false"
        @cancelado="alerta = false"
        titulo="Alerta"
      >¿Estas seguro que quieres salir?</mi-alerta>
      <mi-tabla-estudiantes>Datos de estudiantes</mi-tabla-estudiantes>
      <div style="text-align: center">
        <button class="btn btn-primary" @click="alerta = true">Salir</button>
      </div>
    </div>
  </div>
</template>

<script>
import miAlerta from "@/components/MiAlerta.vue";
import miTablaEstudiantes from "@/components/MiTablaEstudiantes.vue";

export default {
  name: "App",
  components: {
    miAlerta,
    miTablaEstudiantes
  }
};
</script>

<style></style>
