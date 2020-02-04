<template>
  <v-row justify="center"  dense>
    <v-col cols="12" sm="12" lg="6">
      <v-row>
        <v-col cols="8" md="9">
          <v-text-field dense label="Código de barras" solo />
        </v-col>
        <v-col cols="4" md="3">
          <v-btn color="primary" @click="addProduct()">
            <v-icon>add_circle_outline</v-icon>
            Agregar
          </v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-card> </v-card>
      </v-row>
    </v-col>
    <v-col cols="12" sm="12" lg="4" offset-lg="1">
      <v-card class="pa-2">
        <h3 class="headline text-center">Gema</h3>
        <h4 class="subtitle-1 text-center">Carnicería y abarrotera</h4>
        <v-simple-table dense height="360px" class="mt-3">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Producto</th>
                <th class="text-left">Cant.</th>
                <th>Precio</th>
                <th>Sub. total</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in products" :key="item.id">
                <td>{{ item.name }}</td>
                <td>{{ item.amount }}</td>
                <td>$ {{ item.price }}</td>
                <td>$ {{ item.price * item.amount}}</td>
                <td>
                  <v-btn small icon color="error" @click="removeProduct(item)">
                    <v-icon>remove_circle</v-icon>
                  </v-btn>
                </td>
              </tr>
              
            </tbody>
          </template>
        </v-simple-table>
        <h3 class="text-right pa-3 headline">Total: $ {{ totalVenta }}</h3>
        <v-btn color="success" :disabled="totalVenta <= 0" small block>Finalizar venta</v-btn>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "home",
  data: () => ({
    products: [],
    lista: [
      {
        id: 1,
        name: "Nutri Leche",
        price: 14
      },
      {
        id: 2,
        name: "Aceite patrona",
        price: 20
      },
      {
        id: 3,
        name: "Chiles en vinagre, La constena",
        price: 16
      },
      {
        id: 4,
        name: "Papel de baño, Charmin",
        price: 14
      }
    ]
  }),
  computed: {
    totalVenta() {
      let total = 0;
      this.products.forEach(element => {
        total += (element.price * element.amount);
      });
      return total;
    }
  },
  methods: {
    addProduct() {
      let newProduct = this.lista[Math.floor(Math.random() * 4)];
      let index = this.products.findIndex(x => x.id == newProduct.id);
      if(index != -1){
        this.products[index].amount++;
      }else{
        this.products.push({
          id: newProduct.id,
          name: newProduct.name,
          amount: 1,
          price: newProduct.price
        });
      }
    },
    removeProduct(item) {
      if(item.amount == 1){
        let index = this.products.findIndex(x => x.id == item.id);
        this.products.splice(index, 1);
      }else{
        item.amount--;
      }
    }
  }
};
</script>
