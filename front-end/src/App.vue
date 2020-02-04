<template>
  <v-app>
    <v-navigation-drawer v-model="model" temporary overflow app>

      <v-list-item dark class="cyan darken-3">
        <v-list-item-content>
          <v-list-item-title class="title">
            Gema
          </v-list-item-title>
           <v-list-item-subtitle>
            versión 0.0.1
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      

      <v-list dense>
        <v-list-item to="/" link>
          <v-list-item-action>
            <v-icon>home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        
        <v-list-item link to="productos">
          <v-list-item-action>
            <v-icon>storage</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Productos</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>


    </v-navigation-drawer>

    <v-app-bar app dark color="cyan darken-4">
      <v-app-bar-nav-icon @click.stop="model = !model">
        <v-icon>menu</v-icon>
      </v-app-bar-nav-icon>
      <v-toolbar-title>Gema</v-toolbar-title>
    </v-app-bar>

    <v-content class="blue-grey lighten-5">
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-content>

    <v-footer dark color="blue-grey darken-4">
      <span class="px-4">&copy; 2020 - Gema</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    model: false
  })
};
</script>

// import HelloWorld from "./components/HelloWorld"; // import Bar from
"./views/AppBar"; // export default { // name: "App", // components: { //
HelloWorld, // Bar // }, // data: () => ({ // // // }) // };
