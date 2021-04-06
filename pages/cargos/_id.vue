<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Edicion Cargos</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/usuario">
          Lista cargos
        </v-btn>
      </v-card-actions>
      <v-form ref="formCargos" v-model="valid" lazy-validation>
        <v-text-field
          v-model="cargo.nombre"
          :rules="nombreRules"
          label="Nombre del cargo"
          required
        >
        </v-text-field>

        <v-text-field
          v-model="cargo.descripcion"
          :rules="descripcionRules"
          label="Descripcion del cargo"
          required
        ></v-text-field>

        <v-text-field
          v-model="cargo.actividades"
          :rules="actividadesRules"
          label="Actividades a desempeñar"
          required
        >
        </v-text-field>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="actualizarCargo()"
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
    let idCargo = params.id;
    return { idCargo };
  },
  data: () => ({
    valid: true,
    cargo: {
      nombre: "",
      descripcion: "",
      actividades: "",
    },
    nombreRules: [(v) => !!v || "documento is required"],
    descripcionRules: [(v) => !!v || "nombre es requerido"],
    actividadesRules: [(v) => !!v || "contraseña es requerida"],
    select: null,
  }),

  beforeMount() {
    this.getCargo();
  },

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo update
     */
    async getCargo() {
      try {
        let response = await this.$axios.get(
          "http://localhost:3001/cargos/" + this.idCargo
        );
        this.cargo = response.data;
      } catch (error) {
        console.error(error);
      }
    },

    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async actualizarCargo() {
      if (this.$refs.formCargos.validate()) {
        console.log("--inicio guardar--");
        let cargo = Object.assign({}, this.cargo);
        console.log(cargo);
        let response = await this.$axios.put(
          "http://localhost:3001/cargos/" + this.idCargo,
          cargo
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