<template>
  <v-data-table
    :headers="headers"
    :items="products"
    :search="search"
    :loading="loading"
    loading-text="Actualizando registros"
    sort-by="calories"
    class="elevation-3"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title class="mr-2">Lista de Productos</v-toolbar-title>
        <v-text-field
          v-model="search"
          append-icon="search"
          class="ml-5"
          single-line
          hide-details
          label="Buscar"
        ></v-text-field>
        <v-spacer></v-spacer>

        <v-dialog
          v-model="dialog"
          fullscreen
          hide-overlay
          transition="dialog-bottom-transition"
        >
          <template v-slot:activator="button">
            <v-tooltip bottom>
              <template v-slot:activator="tooltip">
                <span v-on="tooltip.on">
                  <v-btn color="primary" fab small dark v-on="button.on">
                    <v-icon dark>add</v-icon>
                  </v-btn>
                </span>
              </template>
              <span>Agregar producto</span>
            </v-tooltip>
          </template>

          <v-card>
            <v-toolbar short dense dark color="teal">
              <v-btn icon dark @click="dialog = false">
                <v-icon>close</v-icon>
              </v-btn>
              <v-toolbar-title> {{ formTitle }} </v-toolbar-title>
            </v-toolbar>

            <v-stepper v-model="step" vertical style="box-shadow:none;">
              <v-stepper-step :complete="step > 1" editable step="1">
                Producto
                <small>Por favor, a completa la información</small>
              </v-stepper-step>

              <v-stepper-content step="1">
                <v-card class="px-2 pb-2" style="box-shadow:none;">
                  <v-row>
                    <v-col cols="4">
                      <v-text-field
                        clearable
                        solo
                        label="Nombre del producto"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field clearable solo label="Marca"></v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field
                        clearable
                        solo
                        label="Código de barras"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="4">
                      <v-text-field
                        clearable
                        solo
                        label="Precio"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-select label="Unidad" solo></v-select>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field
                        clearable
                        solo
                        label="Cantidad"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-btn color="primary" @click="step = 2">Siguiente</v-btn>
                </v-card>
              </v-stepper-content>

              <v-stepper-step :complete="step > 2" step="2">
                Ofertas
                <small>Por favor, a completa la información</small>
              </v-stepper-step>
              <v-stepper-content step="2">
                <v-card>
                  <v-btn color="primary" class="mr-3" @click="step = 1">Atras</v-btn>
                  <v-btn color="success">Guardar</v-btn>
                </v-card>
              </v-stepper-content>
            </v-stepper>
          </v-card>
        </v-dialog>

        <!-- <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.name"
                      label="Dessert name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.calories"
                      label="Calories"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.fat"
                      label="Fat (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.carbs"
                      label="Carbs (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog> -->
      </v-toolbar>
    </template>
    <template v-slot:item.action="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)">
        edit
      </v-icon>
      <v-icon small @click="deleteItem(item)">
        delete
      </v-icon>
    </template>

    <template v-slot:no-data>
      <v-btn color="blue" small dark @click="initialize">
        <v-icon>cached</v-icon>
        Refrescar</v-btn
      >
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: "Products",
  data: () => ({
    step: 1,
    search: "",
    dialog: false,
    loading: false,
    headers: [
      {
        text: "ID",
        align: "left",
        sortable: false,
        value: "name"
      },
      { text: "Nombre", value: "calories" },
      { text: "Marca", value: "protein" },
      { text: "Precio", value: "fat" },
      { text: "Cantidad", value: "carbs" },
      { text: "Opciones", value: "action", sortable: false }
    ],
    products: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    },
    defaultItem: {
      name: "",
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    }
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Nuevo Producto" : "Editar Producto";
    }
  },
  watch: {
    dialog(val) {
      val || this.close();
    }
  },
  created() {
    this.initialize();
  },
  methods: {
    initialize() {
      this.desserts = [];
    },
    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },
    deleteItem(item) {
      const index = this.desserts.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.desserts.splice(index, 1);
    },
    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem);
      } else {
        this.desserts.push(this.editedItem);
      }
      this.close();
    }
  }
};
</script>
