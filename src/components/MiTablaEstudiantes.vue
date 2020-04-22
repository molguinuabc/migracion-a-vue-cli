<template>
  <div>
    <h4 style="text-align: center">
      <slot></slot>
    </h4>
    <div class="panel panel-default">
      <dialog-alerta
        v-if="alertaRegistro"
        @ok="alertaRegistro = false"
        @cancelado="alertaRegistro = false"
        titulo="Atención"
      >
        ¿Estás seguro que quieres borrar el registro con matrícula
        {{ matricula }}?
      </dialog-alerta>
      <dialog-alerta
        v-if="alertaCurso"
        @ok="alertaCurso = false"
        @cancelado="alertaCurso = false"
        titulo="Atención"
      >¿Estás seguro que quieres quitarle el curso {{ curso }}?</dialog-alerta>
      <table class="table table-striped table-bordered">
        <thead>
          <tr>
            <th style="text-align: center">Matricula</th>
            <th style="text-align: center">Nombre</th>
            <th style="text-align: center">Cursos</th>
            <th style="text-align: center">
              <button type="button" class="btn btn-primary btn-default btn-sm" title="Nuevo Alumno">
                <span class="glyphicon glyphicon-plus" aria-hidden="false"></span>
              </button>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="e in estudiantes" :key="e">
            <td style="text-align: center">{{ e.matricula }}</td>
            <td style="text-align: center">{{ e.nombre }}</td>
            <td>
              <div style="text-align: end; vertical-align: middle; ">
                <button
                  type="button"
                  class="btn btn-info btn-default btn-sm"
                  title="Inscribir en un curso"
                >
                  <span class="glyphicon glyphicon-plus" aria-hidden="false"></span>
                </button>
              </div>
              <p style="text-align: end" v-for="c in e.cursosNombre" :key="c">
                {{ c }}
                <button
                  type="button"
                  class="btn btn-default btn-sm"
                  :title="'Dar de baja en el curso: ' + c"
                  @click="confirmarBorradoCurso(c)"
                >
                  <span class="glyphicon glyphicon-remove" aria-hidden="false"></span>
                </button>
              </p>
            </td>
            <td>
              <div style="text-align: center; ">
                <button
                  type="button"
                  class="btn btn-danger btn-default btn-sm"
                  :title="'Eliminar registro de matricula ' + e.matricula"
                  @click="confirmarBorradoRegistro(e.matricula)"
                >
                  <span class="glyphicon glyphicon-remove" aria-hidden="false"></span>
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import miAlerta from "@/components/MiAlerta.vue";
export default {
  data: function() {
    return {
      estudiantes: [],
      alertaRegistro: false,
      alertaCurso: false,
      matricula: 0,
      curso: ""
    };
  },
  methods: {
    confirmarBorradoRegistro: function(m) {
      this.matricula = m;
      this.alertaRegistro = true;
    },
    confirmarBorradoCurso: function(c) {
      this.curso = c;
      this.alertaCurso = true;
    }
  },
  components: {
    dialogAlerta: miAlerta
  },
  mounted: function() {
    fetch("http://localhost:4000/estudiantes", {
      method: "GET",
      headers: {
        "Content-Type": "application/json"
      }
    })
      .then(response => {
        return response.json();
      })
      .then(datos => {
        datos.forEach(e => {
          if (!e.cursosNombre) e.cursosNombre = [];
          e.cursos.forEach(c => {
            fetch(c, {
              method: "GET",
              headers: {
                "Content-Type": "application/json"
              }
            })
              .then(respCurso => {
                return respCurso.json();
              })
              .then(datoCurso => {
                e.cursosNombre.push(datoCurso.nombre);
              })
              .catch(error => {
                console.log(error);
              });
          });
        });
        this.estudiantes = datos;
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

<style></style>
