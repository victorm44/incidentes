<template>
  <v-app>
    <!--Menu hamburguesa-->
    <v-navigation-drawer
      v-model="openMenu"
      :clipped="clipped"
      fixed 
      app>
      <v-list>
        <v-list-item
          v-for="item in items"
          :key="item.id"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>


    <!-- barra navegacion superior -->
    <v-app-bar :clipped-left="clipped" fixed app>
      
    <!-- icono que controla el menu hamburguesa -->
      <v-app-bar-nav-icon @click.stop="openMenu = !openMenu" />   
    </v-app-bar>
    
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
   
    <v-footer app>
      <span>&copy; {{compania}}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      compania: "team",
      clipped:false,
      openMenu: false,
      items: [
        {
          id: "inicio",
          icon: 'mdi-home',
          title: 'Inicio',
          to: '/'
        },
        {
          id: "incidente",
          icon: 'mdi-alert-box',
          title: 'Incidente',
          to: '/incidentes'
        },
        {
          id: "usuarios",
          icon: 'mdi-account',
          title: 'Usuarios',
          to: '/usuarios'
        },
        {
          id: "cargos",
          icon: 'mdi-briefcase-account',
          title: 'Cargos',
          to: '/cargos'
        },
        {
          id: "datos_empleado",
          icon: 'mdi-card',
          title: 'Datos de empleado',
          to: '/datos_empleado'
        }
      ],
      title: 'Vuetify.js'
    }
  }
}
</script>
