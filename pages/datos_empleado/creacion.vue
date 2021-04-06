<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Crear datos de empleado</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/datos_empleado">
          Lista datos Empleado
        </v-btn>
      </v-card-actions>
      <v-form ref="formDatosEmpleado" v-model="valid" lazy-validation>
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
          label="tipo de incidente"
          required
        ></v-select>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="guardarDatosEmpleado()"
        >
          Guardar
        </v-btn>
      </v-form>
    </v-card>
  </div>
</template>

<script>
export default {
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
    cargos: ["Jefe", "Asistente", "Gerente"],
    checkbox: false,
  }),

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async guardarDatosEmpleado() {
      if (this.$refs.formDatosEmpleado.validate()) {
        console.log("--inicio guardar--");
        let datosEmpleado = Object.assign({}, this.datosEmpleado);
        console.log(datosEmpleado);
        let response = await this.$axios.post(
          "http://localhost:3001/datosEmpleado",
          datosEmpleado
        );
        this.$swal
          .fire({
            type: "success",
            title: "Operacion exitosa",
            text: "Los datos se guardaron con exito",
            footer: "<a href>Why do I have this issue?</a>",
          })
          .then((result) => {
            console.log(result);
            if (result.value) {
              this.$router.push("/cargos");
            }
          });
        console.log(response);
      } else {
        this.$swal.fire({
          type: "warning",
          title: "Rellena todos los datos",
          text: "Por favor adjunte todos los datos solicitados",
          footer: "<a href>Why do I have this issue?</a>",
        });
        console.log("Formulario incompleto");
      }
    },
  },
};
</script>

<style>
</style>