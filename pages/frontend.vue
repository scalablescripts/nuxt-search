<template>
  <Products :products="filteredProducts" :filters="filters" :lastPage="lastPage" @set-filters="filtersChanged($event)"/>
</template>

<script>
import Products from "../components/Products";

export default {
  name: "frontend",
  components: {Products},
  data() {
    return {
      allProducts: [],
      filteredProducts: [],
      filters: {
        s: '',
        sort: '',
        page: 1
      },
      lastPage: 0,
      perPage: 9
    }
  },
  async mounted() {
    const response = await fetch('http://localhost:8000/api/products/frontend');
    const content = await response.json();
    this.allProducts = content;
    this.filteredProducts = content.slice(0, this.filters.page * this.perPage);
    this.lastPage = Math.ceil(content.length / this.perPage);
  },
  methods: {
    filtersChanged(f) {
      this.filters.s = f.s;
      this.filters.sort = f.sort;
      this.filters.page = f.page;

      let products = this.allProducts.filter(p => p.title.toLowerCase().indexOf(this.filters.s.toLowerCase()) >= 0 ||
        p.description.toLowerCase().indexOf(this.filters.s.toLowerCase()) >= 0);

      if (this.filters.sort === 'asc' || this.filters.sort === 'desc') {
        products.sort((a, b) => {
          const diff = a.price - b.price;

          if (diff === 0) return 0;

          const sign = Math.abs(diff) / diff;

          return this.filters.sort === 'asc' ? sign : -sign;
        })
      }

      this.lastPage = Math.ceil(products.length / this.perPage);
      this.filteredProducts = products.slice(0, this.filters.page * this.perPage);
    }
  }
}
</script>
