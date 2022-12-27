<template>
      <div class="container mx-auto">
          <div class="grid grid-cols-12">
            <div class="col-span-12 flex items-center mx-auto justify-center gap-4">
              <select placeholder="Choose brand" v-model="selectedBrand" class="form-select appearance-none  
                block
                w-[300px]
                px-3
                py-3
                text-base
                font-normal
                text-gray-700
                bg-white bg-clip-padding bg-no-repeat
                border border-solid border-gray-300
                rounded
                transition
                ease-in-out
                m-0
                focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none">
                  <option value="all">All</option>  
                  <option v-for="(item,index) in brands" :key="index" :value="item.name">{{item.name}}</option>
              </select>
              <button @click="clearFilter" class="btn bg-[#c71818] p-3 rounded-md text-white flex items-center justify-center w-[200px]">Clear filter</button>
            </div>
          </div>
          <div v-if="!filtered" class="grid grid-cols-12">
            <div class="col-span-3" v-for="(product,index) in displayedPosts" :key="index">
                <product-card :product="product"></product-card>
            </div>
          <div class="col-span-12 flex justify-end clearfix btn-group col-md-2 offset-md-5">
            <button type="button" class="px-3 py-2 ml-0 leading-tight text-gray-500 bg-white border border-gray-300 rounded-l-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white" v-if="page != 1" @click="page--"> prev </button>
            <button type="button" class="px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white" v-for="(pageNumber,index) in pages.slice(page-1, page+5)" :key="index" @click="page = pageNumber"> {{pageNumber}} </button>
            <button type="button" @click="page++" v-if="page < pages.length" class="px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 rounded-r-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"> next </button>
          </div>
          </div>
          <div v-else class="grid grid-cols-12">
            <div class="col-span-3" v-for="(product,index) in filteredPosts" :key="index">
                <product-card :product="product"></product-card>
            </div>
          </div>
      </div>
</template>

<script>
  import axios from 'axios'
  import { data } from '../../public/data.json'
  import ProductCard from '../components/ProductCard.vue'
  export default {
    data(){
      return {
        baseUrl: data,
        page: 1,
        perPage: 4,
        pages: [],
        products: [],
        brands: [],
        selectedBrand: null,
        filteredPosts: [],
        filtered: false
      }
    },
     methods: {
    getProducts () {
      axios.get('/data.json')
        .then(response => {
          this.products = response.data.data;
          this.brands = response.data.brands;
        })
        .catch(error => {
          console.log(error);
        });
    },
    getFilter(){
      this.$router.push({ query: { brand: this.selectedBrand } });
     this.filtered = true;
     if(this.selectedBrand == "all"){
        this.filteredPosts = this.products;
      }
      else {
        this.filteredPosts = this.products.filter((item) => {
          return item.brand.name == this.selectedBrand;
        });
      }
    },
    clearFilter(){
      this.selectedBrand = "all";
      this.filtered = false;
    },
     setPages () {
      this.pages = [];
      let numberOfPages = Math.ceil(this.products.length / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },
    paginate (products) {
      let page = this.page;
      // this.$router.push({ query: { page: page } });
      let perPage = this.perPage;
      let from = (page * perPage) - perPage;
      let to = (page * perPage);
      return  products.slice(from, to);
    }
  },
  created () {
    this.getProducts();
  },
   watch: {
    products () {
      this.setPages();
    },
    selectedBrand () {
      this.getFilter();
    } 
  },
  computed: {
    displayedPosts () {
      return this.paginate(this.products);
    }
  },
  // mounted () {
  //   if (this.$route.query.page) {
  //     this.page = this.$route.query.page;
  //   }
  //   if (this.$route.query.brand) {
  //     this.selectedBrand = this.$route.query.brand;
  //   }
  // },
  components: {
    ProductCard
  }
  }
</script>

<style lang="scss" scoped>

</style>