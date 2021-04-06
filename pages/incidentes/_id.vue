<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Edicion incidente</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/incidentes">
          Lista incidentes
        </v-btn>
      </v-card-actions>
      <v-form ref="formIncidentes" v-model="valid" lazy-validation>
        <v-text-field
          v-model="incidente.horaIncidente"
          :rules="horaRules"
          label="Hora de incidente"
          required
        >
        </v-text-field>

        <v-text-field
          v-model="incidente.idEmpleado"
          :rules="idEmpleadoRules"
          label="Documento Empleado"
          required
        ></v-text-field>

        <v-text-field
          v-model="incidente.descripcion"
          :rules="descripcionRules"
          label="Descripcion del incedente"
          required
        >
          <br />
        </v-text-field>

        <v-select
          v-model="incidente.tipoIncidente"
          :items="tipoIncidente"
          :rules="[(v) => !!v || 'Item is required']"
          label="tipo de incidente"
          required
        ></v-select>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="actualizarIncidente()"
        >
          Modificar
        </v-btn>
      </v-form>
    </v-card>
  </div>
</template>

<script>
export default {
  async asyncData({ params}) {
    console.log(params);
    let idIncidente= params.id;
    return { idIncidente};
  },
  data: () => ({
    valid: true,
    incidente: {
      horaIncidente: "",
      idEmpleado: "",
      descripcion: "",
      tipoIncidente: "",
    },
    idEmpleadoRules: [(v) => !!v || "Name is required"],
    descripcionRules: [(v) => !!v || "Descripcion es requerida"],
    horaRules: [(v) => !!v || "Hora es requerida"],
    select: null,
    tipoIncidente: [
      "Caída de cargas",
      "Choque de vehículos",
      "Fuga, derrame de materiales",
      "Incendio",
    ],
    checkbox: false,
  }),

  beforeMount(){
      this.getIncidente();
  },

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo update
     */
    async getIncidente() {
      try {
        let response = await this.$axios.get(
          "http://localhost:3001/incidentes/"+this.idIncidente
        );
        this.incidente = response.data;
      } catch (error) {
        console.error(error);
      }
    },

    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async actualizarIncidente() {
      if (this.$refs.formIncidentes.validate()) {
        console.log("--inicio guardar--");
        let incidente = Object.assign({}, this.incidente);
        console.log(incidente);
        let response = await this.$axios.put(
          "http://localhost:3001/incidentes/" + this.idIncidente,
          incidente
        );
        this.$swal.fire({
          type: "succes",
          title: "Operacion exitosa",
          text: "se ha actualizado con exito",
          footer: "<a href>Why do I have this issue?</a>",
        })
        console.log(response);
        
      } else {
        this.$swal.fire({
          type: "warning",
          title: "Formulario incompleto",
          text: "Tienes que rellenar todos los datos",
          footer: "<a href>Why do I have this issue?</a>",
        })
        console.log("Formulario incompleto");
      }
    },
  },
};
</script>

<style>
</style>