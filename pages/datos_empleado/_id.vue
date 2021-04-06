<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Edicion datos de Empleado</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/datos_empleado">
          Lista datos de Empleado
        </v-btn>
      </v-card-actions>
      <v-form ref="D" v-model="valid" lazy-validation>
        <v-text-field
          v-model="datosEmpleado.documento"
          :rules="documentoRules"
          label="Documento del empleado"
          required
        >
        </v-text-field>

        <v-text-field
          v-model="datosEmpleado.nombre"
          :rules="nombreRules"
          label="nombre Empleado"
          required
        ></v-text-field>

        <v-text-field
          v-model="datosEmpleado.correo"
          label="Correo del empleado"
        >
        </v-text-field>

        <v-text-field
          v-model="datosEmpleado.fecha"
          :rules="fechaRules"
          label="Fecha de nacimiento del empleado"
          required
        >
          <br />
        </v-text-field>

        <v-select
          v-model="datosEmpleado.cargo"
          :items="cargos"
          :rules="[(v) => !!v || 'Item is required']"
          label="cargo"
          required
        ></v-select>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="guardarDatosEmpleado()"
        >
          Modificar
        </v-btn>
      </v-form>
    </v-card>
  </div>
</template>

<script>
export default {
  async asyncData({ params }) {
    console.log(params);
    let idDatosEmpleado = params.id;
    return { idDatosEmpleado };
  },
  data: () => ({
    valid: true,
    datosEmpleado: {
      documento: "",
      nombre: "",
      correo: "",
      fecha: "",
      cargo: "",
    },
     documentoRules: [(v) => !!v || "Documento is required"],
    nombreRules: [(v) => !!v || "Nombre es requerida"],
    fechaRules: [(v) => !!v || "fecha nacimiento es requerida"],
    select: null,
    cargos: [
      "Jefe",
      "Asistente",
      "Gerente",
    ],
    checkbox: false,
  }),

  beforeMount() {
    this.getDatosEmpleado();
  },

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo update
     */
    async getDatosEmpleado() {
      try {
        let response = await this.$axios.get(
          "http://localhost:3001/datosEmpleado/" + this.idDatosEmpleado
        );
        this.datosEmpleado= response.data;
      } catch (error) {
        console.error(error);
      }
    },

    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async actualizarDatosEmpleado() {
      if (this.$refs.formDatosEmpleado.validate()) {
        console.log("--inicio guardar--");
        let datosEmpleado = Object.assign({}, this.datosEmpleado);
        console.log(datosEmpleado);
        let response = await this.$axios.put(
          "http://localhost:3001/datosEmpleado/" + this.idDatosEmpleado,
          datosEmpleado
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