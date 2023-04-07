<template>
  <div class="card m-4">
    <img class="card-img-top" v-bind:src="lesson.image" alt="Lesson image" />
    <div class="card-body">
      <h4 class="card-title" style="text-align: center">
        {{ lesson.subject }}
      </h4>
      <p class="card-text" style="margin-top: 10px">
        <span><i class="fa-solid fa-location-dot"></i></span>
        Location: {{ lesson.location }}
      </p>
      <p class="card-text">
        <span>
          <i class="fa-solid fa-money-bill"></i>
        </span>
        Price: {{ lesson.price }}
      </p>
      <p class="card-text">
        <span v-if="lesson.spaces === cartCount(lesson.id)">
          <i style="color: red" class="fa-solid fa-xmark"></i>
        </span>
        <span v-else-if="lesson.spaces - cartCount(lesson.id) < 2">
          <i style="color: yellow" class="fa-solid fa-exclamation"></i>
        </span>
        <span v-else>
          <i style="color: green" class="fa-solid fa-check"></i>
        </span>
        Spaces Left: {{ lesson.spaces }}
      </p>
      <div style="text-align: center">
        <!-- <div style="text-align: right; margin-bottom: 15px">
          <span v-for="(n, index) in lesson.rating" :key="index"
            ><i style="color: #fcc404" class="fa-solid fa-star"></i
          ></span>
          <span v-for="(n, index) in 5 - lesson.rating" :key="index"
            ><i class="fa-regular fa-star"></i
          ></span>
        </div> -->
        <button
          class="btn btn-warning"
          v-on:click="addLesson(lesson)"
          v-if="canAddToCart(lesson)"
        >
          Add to cart
        </button>
        <button class="btn btn-warning" disabled="disabled" v-else>
          Add to cart
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LessonVue",
  props:['lesson','cart'],
  methods:{
    addLesson(lesson){
        this.$emit('addToCart',lesson)
    },
    // canAddToCart(lesson) checks if lesson can be added
    canAddToCart(lesson) {
      // if spaces is greater than the count of lesson id
      return lesson.spaces > this.cartCount(lesson._id);
    },
    // returns count of cart items
    cartCount(id) {
      let count = 0;
      // loop thourgh cart length
      for (let i = 0; i < this.cart.length; i++) {
        // checks the id in cart
        if (this.cart[i] === id) {
          // increase count
          count++;
        }
      }
      return count;
    }
  }
};
</script>

<style>
</style>