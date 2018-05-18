<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      fixed
      app
    >
      <v-list dense>
        <v-list-tile @click="menuLink(unit.link)" v-for="unit in sidemenu" :key="unit.order">
          <v-list-tile-action>
            <v-icon>{{unit.icon}}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>{{unit.title}}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar color="dark" dark fixed app>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>my Application</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <v-container fluid fill-height>
        <v-layout
          justify-center
          align-center
        >
          <v-flex text-xs-center>
            <router-view />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
    <v-footer color="dark" dark app>
      <span class="white--text">&copy; 2018 Soysauce</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: null,
      sidemenu: [
        {
          order: 1,
          icon: 'home',
          title: 'home',
          link: '/'
        },
        {
          order: 2,
          icon: 'contact_mail',
          title: 'Test',
          link: '/test'
        }
      ]
    }),
    props: {
      source: String
    },
    methods: {
      menuLink(to) {
        this.$router.push(to)
      }
    }
  }
</script>
