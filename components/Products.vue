<template>
  <div class="album py-5 bg-light">
    <div class="container">

      <div class="col-md-12 mb-4 input-group">
        <input class="form-control" placeholder="Search" @keyup="search($event.target.value)"/>
        <div class="input-group-append">
          <select class="form-select" @change="sort($event.target.value)">
            <option>Select</option>
            <option value="asc">Price Ascending</option>
            <option value="desc">Price Descending</option>
          </select>
        </div>
      </div>

      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
        <div class="col" v-for="product in products" :key="product.id">
          <div class="card shadow-sm">
            <img :src="product.image" height="200"/>

            <div class="card-body">
              <p class="card-text">{{ product.title }}</p>
              <div class="d-flex justify-content-between align-items-center">
                <small class="text-muted">${{ product.price }}</small>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="d-flex justify-content-center mt-4" v-if="filters.page < lastPage">
        <button class="btn btn-primary" @click="loadMore">Load More</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Products",
  props: ['products', 'filters', 'lastPage'],
  methods: {
    search(s) {
      this.$emit('set-filters', {
        ...this.filters,
        s,
        page: 1
      });
    },
    sort(sort) {
      this.$emit('set-filters', {
        ...this.filters,
        sort,
        page: 1
      });
    },
    loadMore() {
      this.$emit('set-filters', {
        ...this.filters,
        page: this.filters.page + 1
      });
    }
  }
}
</script>
