<script setup>
  import { ref } from 'vue';
  import { useRoute } from 'vue-router';
  import { useProductStore } from '@/stores/product';
  
  const route = useRoute();
  const productStore = useProductStore();
  const product = ref(null);
  
  product.value = productStore.products.find(p => p.id === parseInt(route.params.id));
  </script>

  <template>
        <div class="container mt-4">
      <div class="row">
        <div class="col-md-6">
          <img :src="product.img" class="img-fluid" alt="...">
        </div>
        <div class="col-md-6">
            <h2>{{ product.name }}</h2>
            <p>{{ product.description }}</p>
            <p>Price: {{ product.price }}$</p>
            <p>Date of addition: {{ product.publish_at }}</p>
            <button type="button" class="btn btn-danger me-2" @click="addToCart">Add to cart</button>
            <router-link to="/" class="btn btn-primary me-2">Back to Home</router-link>
        </div>
      </div>
    </div>
    </template>
<style>
.container {
    color: #000000;
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 50px 50px rgba(26, 1, 85, 0.1);

}


.btn-danger {
    background-color: rgb(130, 0, 252);
    border-color: rgb(35, 20, 241);
}
.col-md-6{
  background-color: rgb(222, 233, 252);
}
</style>