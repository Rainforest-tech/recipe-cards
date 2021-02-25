<template>
  <div id="app" class="bg-yellow-100 py-12">
    <div class="container  mx-auto px-4 md:px-12">
      <div class="flex flex-wrap -mx-1 lg:-mx-4">
        <div v-for="product in meals" :key="product.id"
             class="my-4 px-1 w-full md:w-1/2 lg:my-4 lg:px-4 lg:w-1/3">
          <product-card-component
            :product="product"
            :activeMeals="activeMeals"
            @loadDetails="getDetails"
            @hideDetails="hideDetails"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import ProductCardComponent from '@/components/ProductCardComponent.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: { ProductCardComponent },
  data() {
    return {
      meals: [],
      activeMeals: [],
      // products: [
      //   {
      //     Name: 'Cheese',
      //     Price: 2.50,
      //     Location: 'Refrigerated foods',
      //     image: 'https://images.unsplash.com/photo-1486297678162-eb2a19b0a32d?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1353&q=80',
      //   },
      //   {
      //     Name: 'Crisps',
      //     Price: 3,
      //     Location: 'the Snack isle',
      //     image: 'https://images.unsplash.com/photo-1604565011092-c0fa4416f80f?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80',
      //   },
      //   {
      //     Name: 'Pizza',
      //     Price: 4,
      //     Location: 'Refrigerated foods',
      //     image: 'https://images.unsplash.com/photo-1513104890138-7c749659a591?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80',
      //   },
      //   {
      //     Name: 'Chocolate',
      //     Price: 1.50,
      //     Location: 'the Snack isle',
      //     image: 'https://images.unsplash.com/photo-1611509964296-0724e9fbe7b2?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80',
      //   },
      //   {
      //     Name: 'Self-raising flour',
      //     Price: 1.50,
      //     Location: 'Home baking',
      //     image: 'https://images.unsplash.com/photo-1570486463987-0cc8e1aed502?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1465&q=80',
      //   },
      //   {
      //     Name: 'Ground almonds',
      //     Price: 3,
      //     Location: 'Home baking',
      //     image: 'https://images.unsplash.com/photo-1565463830503-c03dd8490723?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1267&q=80',
      //   },
      // ],
    };
  },
  beforeMount() {
    this.getData();
  },
  methods: {
    getData() {
      axios.get('https://www.themealdb.com/api/json/v1/1/filter.php?c=Seafood')
        // eslint-disable-next-line no-return-assign
        .then(({ data }) => {
          this.meals = data.meals.map((meal) => ({
            name: meal.strMeal,
            image: meal.strMealThumb,
            id: meal.idMeal,
          }));
        });
    },
    getDetails(id) {
      const index = this.meals.findIndex((meal) => meal.id === id);
      if (!this.meals[index].details) {
        axios.get(`https://www.themealdb.com/api/json/v1/1/lookup.php?i=${id}`)
          .then(({ data }) => {
            this.$set(this.meals[index], 'details', data.meals[0]);
          });
      }
      this.activeMeals.push(id);
    },
    hideDetails(id) {
      this.activeMeals = this.activeMeals.filter((item) => item !== id);
    },
  },
};
</script>

<style>
article {
  min-height: 29.25rem;
}
</style>
