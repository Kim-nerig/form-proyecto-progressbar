<template>
  <div class="row">

    <div class="col-12 mb-4">
      <progress-bar :porcentaje="porcentaje" />
    </div>


    <div class="col-12 col-md-4">
      <form @submit.prevent="registrarProyecto">
        <div class="mb-3">
          <label class="form-label">Proyecto</label>
          <input
            v-model.trim="proyecto"
            type="text"
            class="form-control"
            required
          />
        </div>
        <div class="mb-3">
          <label class="form-label">Actividad</label>
          <select v-model="tipo" class="form-selected" required>
            <option disabled selected value="">
              Selecciona un tipo de actividad
            </option>
            <option>Aplicaciones WEb con Vue.js</option>
            <option>Backend Services con Node.js</option>
            <option>App móvil con React Native</option>
          </select>
        </div>
        <div class="mb-3">
          <label class="form-check-label">Urgentee. .</label>
          <input v-model="urgente" class="form-check-input" type="checkbox" />
        </div>
        <button type="submit" class="btn btn-primary">Guardar</button>
      </form>
    </div>
    <div class="col-12 col-md-8">
      <total-proyectos :numeroProyectos="numeroProyectos" 
      :proyectos="proyectos"
      :cambiarEstado="cambiarEstado" />
    </div>
  </div>
</template>

<script>
import ProgressBar from './ProgressBar.vue';
import TotalProyectos from './TotalProyectos.vue';
export default {
    data: () => ({
        proyecto: "",
        tipo: "",
        urgente: false,
        proyectos: [],
    }),
    methods: {
        registrarProyecto() {
            const proyectos = {
                proyecto: this.proyecto,
                tipo: this.tipo,
                urgente: this.urgente,
                completado: false,
            };
            this.proyectos.push(proyectos);
            localStorage.setItem("proyectos", JSON.stringify(this.proyectos));

            this.proyecto = "";
            this.tipo = "";
            this.urgente = false;
        },
        cambiarEstado(proyecto, campo) {
            //this.proyectos[id].urgente = !this.proyectos[id].urgente;
            //console.log(proyecto, campo);
            proyecto[campo] = !proyecto[campo];
        },
    },
    computed: {
        numeroProyectos() {
            return this.proyectos.length;
        },
        porcentaje() {
            let completados = 0;
            this.proyectos.map(proyecto => {
                if (proyecto.completado)
                    completados++;
            });
            return (completados * 100) / this.numeroProyectos || 0;
        },
    },
    components: { ProgressBar, TotalProyectos },
    mounted () {
      this.proyectos = JSON.parse(localStorage.getItem("proyectos")) || [];
    }
};
</script>