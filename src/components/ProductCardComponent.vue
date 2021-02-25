<template class="">
    <!-- Article -->
    <article class="overflow-hidden rounded-lg shadow-lg bg-yellow-50">

      <a href="#">
        <img alt="Placeholder"  class="block w-full" :src="product.image">
      </a>

      <header class="flex items-center justify-between leading-tight p-2 md:p-4">
        <h1 class="text-lg font-bold">
          <a class="no-underline hover:underline text-black" href="#">
            {{ product.name }}
          </a>
        </h1>
<!--        <p class="text-grey-darker text-xl font-bold">-->
<!--          {{ product.Price }} $-->
<!--        </p>-->
<transition name="fade" mode="out-in">
        <button v-if="!activeMeals.includes(product.id)"
          @click.prevent="$emit('loadDetails', product.id)"
          class="bg-indigo-300 text-white px-3 py-2 rounded-md text-sm font-medium">Details
        </button>
        <button v-else
                @click.prevent="$emit('hideDetails', product.id)"
                class="bg-blue-300 text-white px-3 py-2 rounded-md text-sm font-medium">
          Hide details
        </button>
</transition>
      </header>

      <footer class="flex items-center justify-between leading-none p-2 md:p-4">
        <a class="flex items-center no-underline hover:underline text-black" href="#">
          <img alt="Placeholder"
               class="block rounded-full" height="28" width="28" :src="product.image">
          <p class="ml-2 text-sm">
            {{ product.Location }}
          </p>
        </a>

      </footer>
<transition name="slide">
      <div v-if="activeMeals.includes(product.id)" class="px-4 py-2">
        {{product.details.strInstructions}}
      </div>
</transition>
    </article>
    <!-- END Article -->

</template>

<script>
export default {
  name: 'ProductCardComponent',
  props: {
    product: {
      required: true,
      type: Object,
    },
    activeMeals: {
      required: true,
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style scoped>
.slide-enter-active {
  -moz-transition-duration: 0.3s;
  -webkit-transition-duration: 0.3s;
  -o-transition-duration: 0.3s;
  transition-duration: 0.3s;
  -moz-transition-timing-function: ease-in;
  -webkit-transition-timing-function: ease-in;
  -o-transition-timing-function: ease-in;
  transition-timing-function: ease-in;
}

.slide-leave-active {
  -moz-transition-duration: 0.3s;
  -webkit-transition-duration: 0.3s;
  -o-transition-duration: 0.3s;
  transition-duration: 0.3s;
  -moz-transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  -webkit-transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  -o-transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
}

.slide-enter-to, .slide-leave {
  max-height: 100px;
  overflow: hidden;
}

.slide-enter, .slide-leave-to {
  overflow: hidden;
  max-height: 0;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
