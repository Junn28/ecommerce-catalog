<template>
  <main>
    <div v-show="category === 'men\'s clothing' || category === 'women\'s clothing'" class="bg-product" :class="category === 'men\'s clothing' ? { men_color_2: active } : { women_color_2: active }">
      <div class="container d-flex">
        <div class="loader"></div>
        <div class="col-1 none">
          <img :src="products.image" alt="image-product" />
        </div>
        <div class="col-2 none">
          <h2 class="name-product" :class="category === 'men\'s clothing' ? { text_men: active } : { text_women: active }">{{ products.title }}</h2>
          <div class="category">
            <p>{{ category }}</p>
            <div class="rating">
              <p>{{ rating.rate }}/5</p>
              <div
                v-for="(circle, index) in circles"
                :key="index"
                class="circle"
                :class="[
                  circle <= Math.round(rating.rate) ? (category === 'men\'s clothing' ? { men_color_1: active } : { women_color_1: active }) : '',
                  category === 'men\'s clothing' ? { border_men_1: active } : { border_women_1: active },
                ]"
              ></div>
            </div>
          </div>
          <hr />
          <p class="description">
            {{ products.description }}
          </p>
          <hr />
          <h2 class="price" :class="category === 'men\'s clothing' ? { text_men: active } : { text_women: active }">$ {{ products.price }}</h2>
          <div class="btn-grup">
            <button class="buy" :class="category === 'men\'s clothing' ? { men_color_1: active, border_men_2: active } : { women_color_1: active, border_women_2: active }">Buy now</button>
            <button class="next" :class="category === 'men\'s clothing' ? { text_men: active, border_men_2: active } : { text_women: active, border_women_2: active }" @click="next">Next product</button>
          </div>
        </div>
      </div>
    </div>
    <div v-show="category === 'jewelery' || category === 'electronics'" class="bg-product gray">
      <div class="container h-100">
        <div class="icon-frown">
          <p class="unavailable">This product is unavailable to show</p>
          <button class="next-unavailable" @click="nextProduct">Next product</button>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import '../assets/page.css';

const Api_Url = 'https://fakestoreapi.com/products/';

export default {
  name: 'ProductDisplay',
  data() {
    return {
      currentProduct: 1,
      products: '',
      rating: '',
      category: '',
      circles: [1, 2, 3, 4, 5],
      active: true,
    };
  },

  mounted() {
    // fetch on init
    setTimeout(() => {
      this.loader();
    }, 2000);
    this.fetchData();
  },

  watch: {
    // re-fetch whenever currentBranch changes
    currentProduct: 'fetchData',
  },

  methods: {
    async fetchData() {
      const url = `${Api_Url}${this.currentProduct}`;
      this.products = await (await fetch(url)).json();
      this.rating = this.products.rating;
      this.category = this.products.category;
    },
    next() {
      this.currentProduct++;
      if (this.currentProduct > 20) {
        this.currentProduct = 1;
      }
      setTimeout(() => {
        this.loader();
      }, 2000);
      this.loader();
    },
    nextProduct() {
      this.currentProduct++;
    },
    loader() {
      const loader = document.querySelector('.loader');
      const col1 = document.querySelector('.col-1');
      const col2 = document.querySelector('.col-2');

      loader.classList.toggle('none');
      col1.classList.toggle('none');
      col2.classList.toggle('none');
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
:root {
  --men-color-1: #002772;
  --men-color-2: #d6e6ff;
  --women-color-1: #720060;
  --women-color-2: #fde2ff;
  --light: #fff;
}

.men_color_1 {
  background-color: var(--men-color-1);
}
.men_color_2 {
  background-color: var(--men-color-2);
}
.women_color_1 {
  background-color: var(--women-color-1);
}
.women_color_2 {
  background-color: var(--women-color-2);
}
.border_men_2 {
  border: 2px solid var(--men-color-1);
}
.border_women_2 {
  border: 2px solid var(--women-color-1);
}
.border_men_1 {
  border: 1px solid var(--men-color-1);
}
.border_women_1 {
  border: 1px solid var(--women-color-1);
}
.text_men {
  color: var(--men-color-1);
}
.text_women {
  color: var(--women-color-1);
}
</style>
