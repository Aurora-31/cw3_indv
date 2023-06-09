<template>
  <!-- Mounting the app -->
  <div id="app">
    <!-- Header tag to contain the navbar -->
    <header>
      <nav class="navbar fixed-top navbar-expand-sm navbar-light bg-light py-3">
        <div class="container-fluid">
          <!-- Website Logo -->
          <img
            src="https://see.fontimg.com/api/renderfont4/ALWWx/eyJyIjoiZnMiLCJoIjo5OCwidyI6MTUwMCwiZnMiOjY1LCJmZ2MiOiIjMDAwMDAwIiwiYmdjIjoiI0ZGRkZGRiIsInQiOjF9/Qm9va21hcms/urubus-personal-use-bold.png"
            alt="Title fonts"
            width="160"
          />

          <!-- Input for search. 
              v-model derivative helps bind the value entered by the user. Each key pressed will trigger the search function -->
          <input
            class="form-control ms-auto"
            style="width: 250px"
            type="text"
            placeholder="Search"
            v-model="searchInput"
            v-on:keyup="search"
          />

          <!-- Filter button to show Bootstrap modal with sort options -->
          <!-- v-if derivative enables button only when the showLesson returns true -->
          <button
            v-if="showLesson"
            class="btn btn-outline-dark btn-sm ms-auto"
            data-bs-toggle="modal"
            data-bs-target="#filterModal"
          >
            Filter
          </button>

          <!-- Disabled filter button if showLesson value returns false -->
          <button v-else disabled class="btn btn-outline-dark btn-sm ms-auto">
            Filter
          </button>

          <!-- Checkout button -->
          <!-- Enables if the cart length is more than 0, i.e., when the user adds lessons to cart -->
          <button
            class="btn btn-warning btn-sm ms-3"
            v-on:click="showCheckout"
            v-if="this.cart.length > 0"
          >
            <!-- 'cartItemCount' is used the same way as a data property. -->
            {{ cartItemCount }}
            <!-- add the cart icon -->
            <span class="fa-solid fa-cart-shopping"></span> Checkout
          </button>

          <!-- Disabled checkout button if cart is empty -->
          <button
            class="btn btn-warning btn-sm ms-3"
            disabled="disabled"
            v-else
          >
            <span class="fa-solid fa-cart-shopping"></span> Checkout
          </button>
        </div>
      </nav>
    </header>

    <!-- Modal created for filter options -->
    <div id="filterModal" class="modal" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Choose filter</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>

          <!-- Modal body with sort options -->
          <div class="modal-body">
            <!-- Sort by options -->
            <!-- v-model binds the value selected by user to filterOption -->
            <p>Sort By:</p>
            <div class="form-check">
              <!-- v-bind:value binds the value of each radio button -->
              <input
                v-model="filterOption"
                v-bind:value="'subject'"
                class="form-check-input"
                type="radio"
                name="filter"
                id="subject"
                checked
              />
              <label class="form-check-label" for="subject"> Subject </label>
            </div>
            <div class="form-check">
              <input
                v-model="filterOption"
                v-bind:value="'location'"
                class="form-check-input"
                type="radio"
                name="filter"
                id="location"
              />
              <label class="form-check-label" for="location"> Location </label>
            </div>
            <div class="form-check">
              <input
                v-model="filterOption"
                v-bind:value="'price'"
                class="form-check-input"
                type="radio"
                name="filter"
                id="price"
              />
              <label class="form-check-label" for="price"> Price </label>
            </div>
            <div class="form-check">
              <input
                v-model="filterOption"
                v-bind:value="'spaces'"
                class="form-check-input"
                type="radio"
                name="filter"
                id="availability"
              />
              <label class="form-check-label" for="availability">
                Availability
              </label>
            </div>
            <div class="form-check">
              <input
                v-model="filterOption"
                v-bind:value="'rating'"
                class="form-check-input"
                type="radio"
                name="filter"
                id="rating"
              />
              <label class="form-check-label" for="rating"> Rating </label>
            </div>
            <hr />

            <!-- Order by options -->
            <p>Order by:</p>
            <div class="form-check">
              <input
                v-model="sortOption"
                v-bind:value="'ascending'"
                class="form-check-input"
                type="radio"
                name="sort"
                id="ascending"
                checked
              />
              <label class="form-check-label" for="ascending">
                Ascending
              </label>
            </div>
            <div class="form-check">
              <input
                v-model="sortOption"
                v-bind:value="'descending'"
                class="form-check-input"
                type="radio"
                name="sort"
                id="descending"
              />
              <label class="form-check-label" for="descending">
                Descending
              </label>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-warning"
              data-bs-dismiss="modal"
            >
              Done
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- main tag contains all necessary display divs -->
    <main class="container">
      <h1
        class="display-5"
        v-text="sitename"
        style="text-align: center; font-weight: 400"
      ></h1>

      <!-- Div to display search results -->
      <!-- v-if uses the return value of checkSearchBar to show or hide div -->
      <div v-if="checkSearchBar">
        <h2 class="display-7 mt-3">Search Results</h2>
        <div class="row">
          <!-- v-for is used to loop through the searchResults. Each object is treated like a lesson -->
          <div
            class="col-12 col-md-6 col-lg-4"
            v-for="lesson in searchResults"
            :key="lesson._id"
          >
          <!-- lesson-vue component to show the search result lessons -->
            <lesson-vue
              :lesson="lesson"
              :cart="cart"
              @addToCart="addToCart"
            ></lesson-vue>
          </div>
        </div>
      </div>
      <div v-else>
        <!-- Display lessons div if showLesson returns true -->
        <div v-if="showLesson">
          <div class="row">
            <h1
              class="display-5 mt-3"
              style="text-align: center; font-weight: 400"
            >
              After School Lessons
            </h1>

            <!-- Loop through the sortedLessons array -->
            <div
              class="col-12 col-md-6 col-lg-4"
              v-for="lesson in sortedLessons"
              :key="lesson._id"
            >
            <!-- lesson-vue component for displaying all lessons -->
              <lesson-vue
                :lesson="lesson"
                :cart="cart"
                @addToCart="addToCart"
              ></lesson-vue>
            </div>
          </div>
        </div>

        <!-- Else display checkout page -->
        <div v-else>
          <h2
            class="display-5 mt-3"
            style="text-align: center; font-weight: 400"
          >
            Checkout Page
          </h2>
          <div class="row">
            <!-- Loop through lesson objects sorted in cart -->
            <!-- Setting key to get unqiue value from concatenating index and lesson._id from mongoDB -->
            <div
              class="col-12 col-md-6 col-lg-4"
              v-for="(lesson, index) in cart"
              :key="index + ' ' + lesson._id"
            >
            <!-- Checkout-vue component added for displaying cart products on the checkout page -->
              <checkout-vue
                :lesson="lesson"
                :index="index"
                @remove="remove"
              ></checkout-vue>
            </div>
          </div>

          <!-- Checkout details -->
          <p class="display-6" style="text-align: center; font-weight: 200">
            Checkout Details
          </p>
          <div style="text-align: center; margin: 2%">
            <p style="display: inline">
              <strong>Name:</strong>

              <!-- This input field is bound to 'name'. Trim is used to remove any extra apces -->
              <input v-model.trim="name" />
            </p>
            <p style="display: inline; margin: 2%">
              <strong>Phone number:</strong>

              <!-- v-model binds value -->
              <input v-model="phone" type="number" />
            </p>
          </div>
          <div style="text-align: center; margin-bottom: 2%">
            <!-- Enable button if the condition satisfies -->
            <!-- RegEx is used to check if only letter are entered for name, only numbers are inputted for phone, and its length is equal to 10 -->
            <button
              v-if="
                /^(?!\s*$)[a-zA-Z.+\s'-]+$/.test(this.name) &&
                /^(?!\s*$)[0-9.+\s'-]+$/.test(this.phone) &&
                this.phone.length === 10
              "
              v-on:click="place_order"
              class="btn btn-outline-dark"
              data-bs-toggle="modal"
              data-bs-target="#orderModal"
            >
              Checkout
            </button>

            <!-- Otherwise show disabled button -->
            <button disabled="disabled" v-else class="btn btn-outline-dark">
              Checkout
            </button>
          </div>

          <!-- Modal created to display order submitted message -->
          <div id="orderModal" class="modal" tabindex="-1">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title">Order Submitted</h5>
                  <button
                    type="button"
                    class="btn-close"
                    data-bs-dismiss="modal"
                    aria-label="Close"
                  ></button>
                </div>
                <div class="modal-body">
                  <p>Thank you for placing the order.</p>
                </div>
                <div class="modal-footer">
                  <!-- On click of the okay button in the modal, trigger the after_order function -->
                  <button
                    type="button"
                    class="btn btn-secondary"
                    data-bs-dismiss="modal"
                    v-on:click="after_order"
                  >
                    Okay
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Footer -->
    <footer
      class="d-flex justify-content-center align-items-center py-3 border-top bg-light"
    >
      <div class="text-center">
        <span class="text-muted">Khadija Nadeem Shaikh</span>
      </div>
    </footer>
  </div>
</template>

<script>
/* eslint-disable */
// import Lesson component
import LessonVue from "./components/Lesson.vue";
// import Checkout component
import CheckoutVue from "./components/Checkout.vue";
export default {
  // Declaring components
  components: {
    LessonVue,
    CheckoutVue,
  },
  data() {
    return {
      sitename: "After School Lessons",
      lessons: [],
      initial_details: [],
      cart: [],
      showLesson: true,
      checkSearchBar: false,
      searchInput: "",
      searchResults: [],
      name: "",
      phone: "",
      filterOption: "subject",
      sortOption: "ascending",
    };
  },
  methods: {
    // place_order method to push orders into order collection and update lesson spaces in lesson collection
    place_order() {
      // creating an orderedLesson array from Map() function to replicate the array for each item for the cart without changing the original array
      let orderedLessons = [
        ...new Map(this.cart.map((item) => [item["_id"], item])).values(),
      ];

      // Creating a new array to store the unique lesson ids
      let new_arr = [];

      // Looping through the orderedLessons
      orderedLessons.forEach((item) => {
        // Creating an object to store the ordered lesson details
        let test_obj = {};
        // Looping through the initial values in from the database
        this.initial_details.forEach((old_item) => {
          // if the current values from vue matches the initial value from database
          if (item._id === old_item._id) {
            // Add the id, spaces ordered, and spaces left in the object.
            test_obj = {
              id: item._id,
              lesson_name: item.subject,
              spaces: old_item.spaces - item.spaces,
              spaces_left: old_item.spaces - (old_item.spaces - item.spaces),
            };
          }
        });
        // Push the object in the array
        new_arr.push(test_obj);
      });

      // Creating an order object with the name, phone, order quantity and ordered lessons
      let order_object = {
        name: this.name,
        phone: this.phone,
        order_quantity: this.cartItemCount,
        lessons: new_arr,
      };

      // Fetch request to post the order_object in order collection
      fetch("http://localhost:3000/collection/order", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(order_object),
      })
        .then((res) => res.json())
        .then((data) => {
          console.log({ Success: data });
        });

      // Looping through the ordered lesson array(new_arr) and then updating each lesson spaces with the available space through fetch
      new_arr.forEach((lesson) => {
        fetch("http://localhost:3000/collection/lesson/" + lesson["id"], {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ spaces: lesson["spaces_left"] }),
        })
          .then((res) => res.json())
          .then((data) => {
            console.log({ Success: data });
          });
      });
    },
    // after_order method to perform location reload after order is placed
    after_order() {
      location.reload();
    },
    // search method to push lessons to searchResults array
    search() {
      // Emptying array with each trigger
      this.searchResults = [];
      // checkSearchBar value to true to show search div
      this.checkSearchBar = true;
      // if searchInput is empty then show lessons div
      if (this.searchInput == "") {
        this.checkSearchBar = false;
        this.showLesson = true;
      }
      // Declaring a variable for this to use in the fetch request
      let webstore=this

      // fetch request to get search results by appending the search input in lower case
      fetch(
        "http://localhost:3000/lesson/" + this.searchInput.toLowerCase()
      ).then(function (response) {
        response.json().then(function (json) {
          // storing results in searchResults array
          webstore.searchResults = json;
          console.log(json);
        });
      });
    },
    // addToCart(lesson) method to add lesson to cart
    addToCart(lesson) {
      // Reduce space with each click
      lesson.spaces--;
      // push lesson to cart
      this.cart.push(lesson);
    },
    // showCheckout method to change value with each click
    showCheckout() {
      // if value is set to true then set to false else set to true
      this.showLesson = this.showLesson ? false : true;
      this.searchInput = "";
      this.checkSearchBar = false;
    },
    // removed(id, index) method to remove lesson
    // index coming from component to get unqiue lesson
    remove(id, index) {
      let arr = this.cart;
      let lessons = this.lessons;

      arr.splice(index, 1);

      // Adding spaces back to the lessons array
      for (let i = 0; i < lessons.length; i++) {
        if (lessons[i]["_id"] == id) {
          lessons[i].spaces = lessons[i].spaces + 1;
        }
      }

      // showing lesson page if cart is empty
      if (this.cart.length === 0) {
        this.showLesson = true;
        this.searchInput = "";
      }
    },
  },
  computed: {
    // cartItemCount returns length of cart
    cartItemCount: function () {
      return this.cart.length || "";
    },
    // sortedLessons method creates array of sorted lessosn
    sortedLessons() {
      let lessonsArr = this.lessons.slice(0);

      // if the filterOption is equal to price
      if (this.filterOption === "price") {
        // If sort is selected to ascending
        if (this.sortOption === "ascending") {
          // return array using sort function by checking prices against each other
          return lessonsArr.sort((a, b) => (a.price > b.price ? 1 : -1));
        } else {
          // Else return array with the opposite
          // Created a copy of the lessons arraydirection of compare
          return lessonsArr.sort((a, b) => (a.price < b.price ? 1 : -1));
        }
      }

      if (this.filterOption === "spaces") {
        if (this.sortOption === "ascending") {
          return lessonsArr.sort((a, b) => (a.spaces > b.spaces ? 1 : -1));
        } else {
          return lessonsArr.sort((a, b) => (a.spaces < b.spaces ? 1 : -1));
        }
      }

      if (this.filterOption === "subject") {
        if (this.sortOption === "ascending") {
          return lessonsArr.sort((a, b) => (a.subject > b.subject ? 1 : -1));
        } else {
          return lessonsArr.sort((a, b) => (a.subject < b.subject ? 1 : -1));
        }
      }

      if (this.filterOption === "location") {
        if (this.sortOption === "ascending") {
          return lessonsArr.sort((a, b) => (a.location > b.location ? 1 : -1));
        } else {
          return lessonsArr.sort((a, b) => (a.location < b.location ? 1 : -1));
        }
      }

      if (this.filterOption === "rating") {
        if (this.sortOption === "ascending") {
          return lessonsArr.sort((a, b) => (a.rating > b.rating ? 1 : -1));
        } else {
          return lessonsArr.sort((a, b) => (a.rating < b.rating ? 1 : -1));
        }
      }
    },
  },
  // created function to get lesson when the page loads
  created: function () {
    console.log("Requesting data from server...");

    // sending a get request to the get all lessons
    fetch("http://localhost:3000/collection/lesson").then((res) => {
      res.json().then((json) => {
        // storing the results in lessons array
        this.lessons = json;
        console.log(json);
      });
    });

    // Sending a get request again to create a duplicate array that store initial details from the database
    fetch("http://localhost:3000/collection/lesson").then((res) => {
      res.json().then((json) => {
        // storing the results in intial_details array
        // This will later be used to compare the spaces as the orginal lessons array get manipulated through user actions
        this.initial_details = json;
        console.log(json);
      });
    });
  },
};
</script>

<style></style>
