<template>
  <div class="home">
    <div class="column is-12">
      <div class="box">
        <div class="columns is-multiline">
          <div class="column is-9 mb-5">
          <h3 class="is-size-4"></h3>
          </div>
          <div class="column is-3 mb-5">
            <div class="buttons">
              <button v-show="previous != null" @click="nextPage(previous)" class="button is-ghost">&#10094;</button>
              <button v-show="next != null" @click="nextPage(next)" class="button is-ghost">&#10095;</button>
              <button @click="getProducts" class="button is-ghost">&#8634;</button>
            </div>
          </div>
          <div class="column is-12">
            <table class="table is-fullwidth">
              <thead>
                <tr>
                  <th>№</th>
                  <th>Заказ №</th>
                  <th>Стоимость, $</th>
                  <th>Стоимость, ₽</th>
                  <th>Срок поставки</th>
                </tr>
              </thead>
              <tbody>
                <tr
                v-for="product in products"
                v-bind:key="product.id"
                >
                  <td>{{ product.id }}</td>
                  <td>{{ product.order_id }}</td>
                  <td>{{ product.usd_price }}</td>
                  <td>{{ product.rub_price }}</td>
                  <td>{{ product.expiration }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HomeView',
  data() {
    return {
      next: null,
      previous: null,
      products: []
    }
  },
  mounted() {
    this.getProducts()
  },
  methods: {
    getProducts () {
      axios
        .get('products/')
        .then(response => {
          this.next = response.data.next
          this.previous = response.data.previous
          this.products = response.data.results
        })
        .catch(error => {
          console.log(JSON.stringify(error))
        })
    },
    nextPage (goto) {
      const inx = goto.indexOf('?')
      const next = goto.substring(38)

      axios
        .get(`products/${next}`)
        .then(response => {
          this.next = response.data.next
          this.previous = response.data.previous
          this.products = response.data.results
        })
        .catch(error => {
          console.log(JSON.stringify(error))
        })
    },
  }
}
</script>
