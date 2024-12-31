<script setup>
import UserLayout from '@/layouts/UserLayout.vue';
import { onMounted, ref } from 'vue';

import { useCartStore } from '@/stores/user/cart';
const cartStore = useCartStore()
const orderData = ref({})

onMounted(() => {
  cartStore.loadCheckout()
  if (cartStore.checkout.oderNumber) {
    orderData.value = cartStore.checkout
  }
})
</script>

<template>
  <UserLayout>
    <div class="border border-base-200 shadow-xl max-w-2xl mx-auto p-8 my-4">
      <div>
        <div class="text-2xl font-bold">Your order is successful!</div>
        <div>Hi {{ orderData.name }}</div>
        <div>เตรียมรอรับสินค้าได้เลย</div>
      </div>
      <div class="divider"></div>
        <div class="grid grid-cols-4 gap-2">
          <div>
            <div class="font-bold">Order Date</div>
            <div>{{ orderData.createDate }}</div>
          </div>
          <div>
            <div class="font-bold">Order Number</div>
            <div>{{ orderData.oderNumber }}</div>
          </div>
          <div>
            <div class="font-bold">Payment Method</div>
            <div>{{ orderData.paymentMethod }}</div>
          </div>
          <div>
            <div class="font-bold">Address</div>
            <div>{{ orderData.address }}</div>
          </div>
        </div>
      <div class="divider"></div>
        <div v-for="product in orderData.products" class="grid grid-cols-4 gap-2 mt-4 item-center">
            <div>
              <img class="w-full" :src="product.imageUrl">
            </div>
            <div>
              {{ product.name }}
            </div>
            <div>
              จำนวน: {{ product.quantity }}
            </div>
            <div>
              ราคารวม: {{ product.price * product.quantity }}
            </div>
        </div>
      <div class="divider"></div>
        <div class="flex justify-between">
          <div>ราคาสินค้าทั้งหมด</div>
          <div>{{ orderData.totalPrice }}</div>
        </div>
        <div class="flex justify-between mt-4">
            <div>ค่าส่ง</div>
            <div>0</div>
        </div>
      <div class="divider"></div>
        <div class="flex justify-between mt-4">
            <div class="font-bold">ราคาทั้งสิ้น</div>
            <div>{{orderData.totalPrice}}</div>
        </div>
      <div class="divider"></div>
      <div>ขอบคุณที่ใช้บริการ</div>
    </div>
  </UserLayout>
</template>
