<template>
  <div class="w-72 group bg-white shadow-md rounded-md my-2 outline-none">
    <a href="#">
      <img :src="product.image" alt="Product" class="h-80 w-72 object-cover rounded-t-xl" />
      <div class="px-4 py-3 w-72">
        <span class="text-gray-400 mr-3 uppercase text-xs">{{product?.brand?.name}}</span>
        <p class="text-lg duration-200 group-hover:text-blue-700 font-bold text-black truncate block capitalize">{{product?.name}}</p>
        <div class="flex items-center">
          <p class="text-lg font-semibold text-black cursor-auto my-3">${{product?.discount}}</p>
          <del>
            <p class="text-sm text-gray-600 cursor-auto ml-2">${{ product?.price }}</p>
          </del>
          <div class="ml-auto"></div>
        </div>
        <div class="flex items-center justify-center">
          <p class="text-lg font-semibold text-black cursor-auto my-3">In stock: </p>
          <p class="text-sm text-gray-600 cursor-auto ml-2">{{ product?.in_stock }}</p>
        </div>
        <div class="flex items-center justify-center">
          <p class="text-lg font-semibold text-black cursor-auto my-3">Weight: </p>
          <p class="text-sm text-gray-600 cursor-auto ml-2">{{ product?.weight }}kg</p>
        </div>
        <div class="flex items-center justify-center">
          <p class="text-lg font-semibold text-black cursor-auto my-3">Artnumber: </p>
          <p class="text-sm text-gray-600 cursor-auto ml-2">{{ product?.art_number }}</p>
        </div>  
      </div>
    </a>
  </div>
</template>

<script>
  export default {
    props: {
      product: {
        type: Object,
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>