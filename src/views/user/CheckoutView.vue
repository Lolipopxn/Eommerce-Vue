<script setup>
import { reactive } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

import { useCartStore } from '@/stores/user/cart';

import UserLayout from '@/layouts/UserLayout.vue';

const cartStore = useCartStore()
const router = useRouter()

const FormData = [
  {
    name: 'Email Address',
    field: 'email'
  },
  {
    name: 'Name',
    field: 'name'
  },
  {
    name: 'Address',
    field: 'address'
  },
  {
    name: 'Note',
    field: 'note'
  }
]

const useFormData = reactive({
  email: '',
  name: '',
  address: '',
  note: ''
})

const payment = () => {
  cartStore.placeOrder(useFormData)
  router.push({name: 'success'})
}
</script>

<template>
  <UserLayout>
    <h1 class="text-3xl font-bold p-5" >Checkout</h1>

    <div class="flex"> 
      <section class="flex-auto w-64 bg-base-200 p-8">
        <label v-for="form in FormData" class="form-control w-full">
          <div class="label">
            <span class="label-text">{{ form.name }}</span>
          </div>
          <input v-model="useFormData[form.field]" type="text" placeholder="Type here" class="input input-bordered w-full" />
        </label>
        <button @click="payment()" class="btn btn-accent mt-8 w-full">ชำระเงิน</button>
      </section>
      <section class="flex-auto w-32 bg-slate-200 px-2">
        <div v-for="item in cartStore.items" class="flex bg-white m-8 py-4">
          <div class="flex-1">
            <img class="w-full p-8" :src="item.imageUrl">
          </div>
          <div class="flex-1">
            <div class="flex flex-col justify-between h-full">
              <div>
                <div><b>{{ item.name }}</b></div>
                <div>จำนวน: {{ item.quantity }}</div>
              </div>
              <RouterLink :to="{name: 'cart'}">Edit</RouterLink>
            </div>
          </div>
        </div>
        <div class="divider"></div>
        <div class="p-4">
          <div><b>Oder Summary</b></div>
          <div class="flex justify-between">
            <div>ราคาสินค้าทั้งหมด</div>
            <div>{{ cartStore.summaryPrice }}</div>
          </div>
          <div class="flex justify-between">
            <div>ต่าส่ง: 0</div>
          </div>
        </div>
        <div class="divider"></div>
        <div class="flex justify-between p-4 mb-4">
            <div>ราคาสินค้าทั้งหมด</div>
            <div>{{ cartStore.summaryPrice }}</div>
        </div>
      </section>
    </div>
  </UserLayout>
</template>
