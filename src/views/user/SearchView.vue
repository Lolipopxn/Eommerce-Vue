<script setup>
import { ref, watch, computed } from 'vue';
import { useRoute, useRouter } from 'vue-router';

import UserLayout from '@/layouts/UserLayout.vue';
import Product from '@/components/Product.vue';

import { useCartStore } from '@/stores/user/cart';
import { useProductStore } from '@/stores/user/product';

const cartStore = useCartStore()
const productStore = useProductStore()
const route = useRoute()
const router = useRouter()

const searchText = ref('')

const addToCart = (product) => {
  cartStore.addToCart(product)
  router.push({name: 'cart'})
}

watch(() => route.query.q, (newSearchText) => {
  searchText.value = newSearchText
}), {immediate: true}

const filterProducts = computed(() => {
  return productStore.filterProducts(searchText.value)
})
</script>

<template>
  <UserLayout>
    <div class="text-3xl m-4">search value: {{ searchText }}</div>
    <Product
      :product="filterProducts"
      :addToCart="addToCart"
    ></Product>
  </UserLayout>
</template>
