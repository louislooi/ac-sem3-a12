<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">人氣餐廳</h1>
    <RestaurantTabs
      :initial-restaurant="restaurant"
      v-for="restaurant in restaurants"
      :key="restaurant.id"
    />
    <hr />
    <!-- RestaurantTabs -->
  </div>
</template>
<script>
import NavTabs from "./../components/NavTabs";
import RestaurantTabs from "./../components/RestaurantTabs";
import restaurantsTop from "./../apis/restaurants";
import { Toast } from "./../utils/helpers";

export default {
  components: {
    NavTabs,
    RestaurantTabs,
  },
  data() {
    return {
      restaurants: [],
    };
  },
  created() {
    this.fetchTop();
  },
  methods: {
    async fetchTop() {
      try {
        const response = await restaurantsTop.getRestaurantsTop();
        const { restaurants } = response.data;
        this.restaurants = restaurants;
        console.log(this.restaurants);
      } catch (error) {
        console.log(error);
        Toast.fire({
          icon: "error",
          title: "無法取得餐廳資料，請稍後再試",
        });
      }
    },
  },
};
</script>
