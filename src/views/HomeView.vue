<script setup>
import { useCategoryStore } from '@/stores/categoty';
import { useProductStore } from '@/stores/product';
import { ref } from 'vue';
import { useCartStore } from '@/stores/cart';
const productsStore = useProductStore();

const categoryStore = useCategoryStore();

const category =  ref(null);
const searchText = ref("");

const products =  ref([]);
products.value = productsStore.products;

const filterCategory = () => {
  products.value = productsStore.filterProductsByCategoryName(category.value,null);

}
const searchName = () => {
  category.value = null;
  products.value = productsStore.filterProductsByCategoryName(null,searchText.value);

}
const resetFilters = () => {
    category.value = null;
    searchText.value = '';
    products.value = productsStore.products; 
};

const cartStore = useCartStore();

</script>

<template>
  <div class="container">
    <div class="row mt-4 my-3">
      <div class="col md-5">
        <select class="form-select" aria-label="Default select example" v-model="category" @change="filterCategory">
          <option selected :value="null">Select category</option>
          <option v-for="category in categoryStore.categories" :value="category" :key="category">{{ category }}</option>
        </select>
      </div>
      <div class="col md-5">
        <input type="text" @input="searchName" class="form-control" aria-label="readonly input example" v-model="searchText">
      </div>
      <div class="col md-2">
        <button type="button" class="btn btn-outline-info" @click="resetFilters">Reset</button>
      </div>
    </div>

    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div class="col" v-for="product in products" :key="product.id">
        <div class="card">
         <td> <img :src="product.img" class="card-img-top" alt="..."> </td>
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">${{ product.price }}</p>
            <p class="card-text">{{ product.category }}</p>
            <p class="card-text">Date:{{ product.publish_at }}</p>
            <div class="card-footer text-end">
              <router-link :to="`/product/${product.id}`" class="btn btn-outline-info me-2">Detail</router-link>
              <button type="button" @click="cartStore.addToCart(product.id)" class="btn btn-outline-success">Cart</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
/* Custom Styles */
.container {
  max-width: 1200px;
}

.card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 40px 40px rgba(204, 0, 255, 0.1);
}

.card-body {
  padding: 20px;
}

.card-footer {
  background-color: #bdf3e5;
}

.card-footer button {
  transition: background-color 0.3s ease;
}

.card-footer button:hover {
  background-color: #28c275;
}

input.form-control,
select.form-select {
  border-radius: 50px;
  font-size: 1.1rem;
  background-color: #46f5c9;
}

button.btn {
  border-radius: 50px;
  font-size: 1.1rem;
  background-color: #c6fcee;
}

button.btn:hover {
  opacity: 0.9;
  
}
</style>