<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Crear Cargo</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/cargos">
          Lista Cargos
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
          label="Descripcion cargo"
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
          @click="guardarCargo()"
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
    cargo: {
      nombre: "",
      descripcion: "",
      actividades: "",
    },
    descripcionRules: [(v) => !!v || "Descripcion is required"],
    nombreRules: [(v) => !!v || "Nombre es requerida"],
    actividadesRules: [(v) => !!v || "Actividades es requerida"],
    select: null,
  }),

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async guardarCargo() {
      if (this.$refs.formCargos.validate()) {
        console.log("--inicio guardar--");
        let cargo = Object.assign({}, this.cargo);
        console.log(cargo);
        let response = await this.$axios.post(
          "http://localhost:3001/cargos",
          cargo
        );
        this.$swal.fire({
          type: "success",
          title: "Operacion exitosa",
          text: "Los datos se guardaron con exito",
          footer: "<a href>Why do I have this issue?</a>",
        })
        .then((result)=>{
          console.log(result);
          if(result.value){
            this.$router.push("/cargos")
          }
        });
        console.log(response);
      } else {
        this.$swal.fire({
          type: "warning",
          title: "Rellena todos los datos",
          text: "Por favor adjunte todos los datos solicitados",
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