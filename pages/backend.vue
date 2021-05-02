<template>
  <Products :products="products" :filters="filters" :lastPage="lastPage" @set-filters="load($event)"/>
</template>

<script>
import Products from "../components/Products";

export default {
  name: "backend",
  components: {Products},
  data() {
    return {
      products: [],
      filters: {
        s: '',
        sort: '',
        page: 1
      },
      lastPage: 0
    }
  },
  async mounted() {
    await this.load(this.filters);
  },
  methods: {
    async load(f) {
      this.filters.s = f.s;
      this.filters.sort = f.sort;
      this.filters.page = f.page;
      const arr = [];

      if (this.filters.s) {
        arr.push(`s=${this.filters.s}`);
      }

      if (this.filters.sort) {
        arr.push(`sort=${this.filters.sort}`);
      }

      if (this.filters.page) {
        arr.push(`page=${this.filters.page}`);
      }

      const response = await fetch(`http://localhost:8000/api/products/backend?${arr.join('&')}`);
      const content = await response.json();
      this.products = this.filters.page === 1 ? content.data : [...this.products, ...content.data];
      this.lastPage = content.last_page;
    }
  }
}
</script>
