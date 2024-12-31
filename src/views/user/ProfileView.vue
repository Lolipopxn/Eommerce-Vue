<script setup>
import { ref, onMounted } from 'vue';
import UserLayout from '@/layouts/UserLayout.vue';

const profileImgUrl = ref('../../../public/Profile.jpg')
const email = ref('')
const name = ref('')

const handleFileUpload = (event) => {
  const file = event.target.files[0]

  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => {
      profileImgUrl.value = e.target.result
    }
    reader.readAsDataURL(file)
  }
}

const updateProfile = () => {
  const profileData = {
    imageUrl: profileImgUrl.value,
    name: name.value,
    email: email.value
  }
  localStorage.setItem('profile-data', JSON.stringify(profileData))
  alert('Success Update Profile')
}

onMounted(() => {
  let profileData = localStorage.getItem('profile-data')
  if (profileData){
    profileData = JSON.parse(profileData)
    profileImgUrl.value = profileData.imageUrl
    name.value = profileData.name
    email.value = profileData.email
  }

})
</script>

<template>
  <UserLayout>
    <div class="border border-base-200 shadow-xl max-w-2xl mx-auto p-8 my-4">
      <div class="font-bold text-2xl">Your Profile</div>
      <div class="flex flex-col items-center">
        <div class="flex flex-col items-center">
          <div class="avatar">
            <div  class="w-24 rounded-full">
              <img :src="profileImgUrl">
            </div>
          </div>
          <input type="file" @change="handleFileUpload">
        </div>
        
        <div class="w-full">
          <label class="form-control w-full">
            <div class="label">
              <span class="label-text">Email</span>
            </div>
            <input v-model="email" type="text" placeholder="Type here" class="input input-bordered w-full" />
          </label>
        </div>
        <div class="w-full">
          <label class="form-control w-full">
            <div class="label">
              <span class="label-text">Name</span>
            </div>
            <input v-model="name" type="text" placeholder="Type here" class="input input-bordered w-full" />
          </label>
        </div>
        <button @click="updateProfile" class="btn btn-accent w-full mt-8">Update Profile</button>
      </div>
    </div>
  </UserLayout>
</template>
