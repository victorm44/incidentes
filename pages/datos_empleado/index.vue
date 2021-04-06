<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-card flat>
        <v-card-actions>
          <h1>Listado Datos de los empleados</h1>
          <v-spacer></v-spacer>
          <v-btn color="success" to="/datos_empleado/creacion"
            >crear empleado</v-btn
          >
        </v-card-actions>
      </v-card>

      <card-text>
        <v-data-table
          :headers="headers"
          :items="datosEmpleado"
          :items-per-page="5"
          class="elevation-1"
        >
          <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-2" @click="editDatosEmpleado(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteDatosEmpleado(item)"> mdi-delete </v-icon>
          </template>
        </v-data-table>
      </card-text>
    </v-col>
  </v-row>
</template>

<script>
export default {
  beforeMount() {
    this.getDatosEmpleado();
  },
  data() {
    return {
      headers: [
        {
          text: "Id",
          align: "start",
          sortable: false,
          value: "id",
        },
        { text: "Documento", value: "documento" },
        { text: "Nombre del empleado", value: "nombre" },
        { text: "Correo", value: "correo" },
        { text: "Fecha de nacimiento", value: "fecha" },
        { text: "Cargo que desempeña", value: "cargo" },
        { text: "Acciones", value: "actions" },
      ],
      datosEmpleado: [],
    };
  },
  methods: {
    /**
     * Enviar una solicitud (Request) en un metodo get
     */
    async getDatosEmpleado() {
      try {
        let response = await this.$axios.get(
          "http://localhost:3001/datosEmpleado"
        );
        this.datosEmpleado = response.data;
      } catch (error) {
        this.$swal.fire({
          type: "error",
          title: "Oops...",
          text: "No se encuentra o no se puede cargar",
          footer: "<a href>Why do I have this issue?</a>",
        })
        .then((result)=>{
          console.log(result);
          if(result.value){
            this.$router.push("/datos_empleado")
          }
        });
        console.error(error);
      }
    },
    async editDatosEmpleado(item) {
        console.log(item);
        let url='/datos_empleado/'+item.id;
        this.$router.push(url);
    },
    async deleteDatosEmpleado(item) {
        this.$swal
        .fire({
          type: "warning",
          title: "Estas seguro?",
          text: "No se podra revertir esta accion",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showCancelButton: true,
        })
        .then(async (result) => {
          if (result.value) {
            try {
              let url = "http://localhost:3001/datosEmpleado/" + item.id;
              await this.$axios.delete(url);
              this.$swal.fire({
                type: "success",
                title: "Operacion exitosa!!",
                text: "Se ha eliminado el item con exito",
                footer: "<a href>Why do I have this issue?</a>",
              });
              this.getDatosEmpleado();
            } catch (error) {
              this.$swal.fire({
                type: "error",
                title: "Ocurrio un problema",
                text: error.toString(),
                footer: "<a href>Why do I have this issue?</a>",
              });
            }
          }
        });
    },
  },
};
</script>