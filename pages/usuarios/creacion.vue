<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Crear Usuario</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/usuarios">
          Lista Usuarios
        </v-btn>
      </v-card-actions>
      <v-form ref="formUsuarios" v-model="valid" lazy-validation>
        <v-text-field
          v-model="usuario.documento"
          :rules="documentoRules"
          label="Documento usuario"
          required
        >
        </v-text-field>

        <v-text-field
          v-model="usuario.nombre"
          :rules="nombreRules"
          label="Nombre usuario"
          required
        ></v-text-field>

        <v-text-field
          v-model="usuario.contra"
          :rules="contraRules"
          label="contraseña"
          required
        >
        </v-text-field>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="guardarUsuario()"
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
    usuario: {
      documento: "",
      nombre: "",
      contra: "",
    },
    documentoRules: [(v) => !!v || "Documento is required"],
    nombreRules: [(v) => !!v || "Nombre es requerida"],
    contraRules: [(v) => !!v || "Contraseñ es requerida"],
    select: null,
  }),

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async guardarUsuario() {
      if (this.$refs.formUsuarios.validate()) {
        console.log("--inicio guardar--");
        let usuario = Object.assign({}, this.usuario);
        console.log(usuario);
        let response = await this.$axios.post(
          "http://localhost:3001/usuario",
          usuario
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