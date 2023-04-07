<template>
                <div class="card m-4">
                <img
                  class="card-img-top"
                  v-bind:src="lesson.image"
                  alt="Lesson image"
                />
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
                  <div style="text-align: center">
                    <!-- Remove button -->
                    <!-- Triggers remove(lesson.id) function -->
                    <button
                      class="btn btn-danger"
                      v-on:click="removeLesson(lesson._id, index)"
                    >
                      Remove
                    </button>
                  </div>
                </div>
              </div>
</template>

<script>
export default {
 name: "CheckoutVue",
  props:['lesson','index'],
  methods:{
    removeLesson(lessonID, index){
        this.$emit('remove',lessonID, index)
    },
  }
}
</script>

<style>

</style>