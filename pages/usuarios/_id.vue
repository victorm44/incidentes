<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Edicion Usuario</h1>
        <v-spacer></v-spacer>
        <v-btn color="secondary" class="text-none" to="/usuario">
          Lista usuarios
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
          :rules="nombredoRules"
          label="Nombre usuario"
          required
        ></v-text-field>

        <v-text-field
          v-model="usuario.contra"
          :rules="contraRules"
          label="Contraseña usuario"
          required
        >
        </v-text-field>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="actualizarUsuario()"
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
    let idUsuario= params.id;
    return { idUsuario};
  },
  data: () => ({
    valid: true,
    usuario: {
      documento: "",
      nombre: "",
      contra: ""
    },
    documentoRules: [(v) => !!v || "documento is required"],
    nombreRules: [(v) => !!v || "nombre es requerido"],
    contraRules: [(v) => !!v || "contraseña es requerida"],
    select: null,
  }),

  beforeMount(){
      this.getUsuario();
  },

  methods: {
    /**
     * // enviar una solicitud (request) en un metodo update
     */
    async getUsuario() {
      try {
        let response = await this.$axios.get(
          "http://localhost:3001/usuario/"+this.idUsuario
        );
        this.usuario = response.data;
      } catch (error) {
        console.error(error);
      }
    },

    /**
     * // enviar una solicitud (request) en un metodo post
     */
    async actualizarIncidente() {
      if (this.$refs.formUsuarios.validate()) {
        console.log("--inicio guardar--");
        let usuario = Object.assign({}, this.usuario);
        console.log(usuario);
        let response = await this.$axios.put(
          "http://localhost:3001/usuario/" + this.idUsuario,
          usuario
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