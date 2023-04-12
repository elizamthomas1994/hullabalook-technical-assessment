<template>
  <div class="app" ref="app-container">
    <div class="filters">
      <h3>Apply filters:</h3>
      <div>
        <label>
          <input type="checkbox" v-model="inStockFilter" />
          Available only
        </label>
      </div>
      <div class="brand-check">
        <label>
          <input type="checkbox" v-model="adidasFilterIsActive" />
          Adidas
        </label>
        <label>
          <input type="checkbox" v-model="nikeFilterIsActive" />
          Nike
        </label>
        <label>
          <input type="checkbox" v-model="newbalanceFilterIsActive" />
          New Balance
        </label>
        <label>
          <input type="checkbox" v-model="converseFilterIsActive" />
          Converse
        </label>
        <label>
          <input type="checkbox" v-model="crocsFilterIsActive" />
          Crocs
        </label>
      </div>
    </div>

    <div class="product-counter">
      <h4>Currently displaying: {{ filteredProducts.length }} products</h4>
    </div>

    <div class="sort-controls">
      <button @click="togglePriceOrder">
        <span v-if="priceAscending">Sort Price Low to High</span>
        <span v-else>Sort Price High to Low</span>
      </button>
    </div>

    <div class="product-grid">
      <ProductGridItem
        v-for="product in filteredProducts"
        :key="product.name"
        :product="product"
        class="product-grid-item"
      />
    </div>
  </div>
</template>

<script>
import ProductGridItem from './components/ProductGridItem.vue';
import products from './data/products.json';

export default {
  name: 'App',
  components: {
    ProductGridItem,
  },
  data() {
    return {
      products,
      inStockFilter: true,
      priceAscending: true,
    };
  },
  computed: {
    filteredProducts() {
      if (this.inStockFilter) {
        return this.products.filter((product) => product.isAvailable);
      } else {
        return this.products;
      }
    },
  },
  methods: {
    togglePriceOrder() {
      const sortedProducts = [...this.filteredProducts].sort(
        (a, b) => a.price - b.price
      );
      this.products = this.priceAscending
        ? sortedProducts
        : sortedProducts.reverse();
      this.priceAscending = !this.priceAscending;
    },
  },
};
</script>

<style>
body {
  background-color: #9ec5e8;
}

.app {
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #000;
}

h3,
h4 {
  color: #fff;
}
.link {
  color: #3a7f71;
}

.product-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.product-grid-item {
  flex-basis: calc(33.33% - 10px);
  margin-bottom: 20px;
}

.sort-controls {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 20px;
}

.sort-controls button {
  background: #fff;
  color: #000;
  border: none;
  border-radius: 5px;
  padding: 10px;
  cursor: pointer;
}
</style>
