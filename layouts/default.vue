<template>
  <div class="container mx-auto">
    <ul class="flex">
      <li class="mr-6">
        <NuxtLink class="text-blue-500 hover:text-blue-800" to="/">Home</NuxtLink>
      </li>
      <li class="mr-6">
        <NuxtLink class="text-blue-500 hover:text-blue-800" to="/posts">Posts</NuxtLink>
      </li>
    </ul>
    <Nuxt />
  </div>
</template>

<script>
export default {  
  setup() {
    
  },
}
</script>