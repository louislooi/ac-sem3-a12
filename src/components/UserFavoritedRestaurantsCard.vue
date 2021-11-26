<template>
  <div class="card">
    <div class="card-header">
      <strong>{{ profile.FavoritedRestaurants.length }}</strong> 收藏的餐廳
    </div>
    <div class="card-body">
      <router-link
        :to="{ name: 'restaurant', params: { id: favRestaurant.id } }"
        class="m-1"
        v-for="favRestaurant in profile.FavoritedRestaurants"
        :key="favRestaurant.id"
        href="#"
      >
        <img :src="favRestaurant.image" width="60" height="60" class="avatar" />
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    initialProfile: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      profile: this.initialProfile,
    };
  },
};
</script>
