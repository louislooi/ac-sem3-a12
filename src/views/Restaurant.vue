<template>
  <div class="container py-5">
    <!-- 餐廳資訊頁 RestaurantDetail -->
    <RestaurantDetail :initial-restaurant="restaurant" />
    <hr />
    <!-- 餐廳評論 RestaurantComments -->
    <RestaurantComments :restaurant-comments="restaurantComments" />
    <!-- 新增評論 CreateComment -->
    <CreateComment :restaurant-id="restaurant.id" />
  </div>
</template>

<script>
//     @after-delete-comment="afterDeleteComment"
import RestaurantDetail from "./../components/RestaurantDetail.vue";
import RestaurantComments from "./../components/RestaurantComments.vue";
import CreateComment from "./../components/CreateComment.vue";
import restaurantsAPI from "./../apis/restaurants";
import { Toast } from "./../utils/helpers";
import { mapState } from "vuex";

export default {
  components: {
    RestaurantDetail,
    RestaurantComments,
    CreateComment,
  },

  data() {
    return {
      restaurant: {
        id: -1,
        name: "",
        categoryName: "",
        image: "",
        openingHours: "",
        tel: "",
        address: "",
        description: "",
        isFavorited: false,
        isLiked: false,
      },
      restaurantComments: [],
    };
  },

  computed: {
    ...mapState(["currentUser"]),
  },
  created() {
    const { id } = this.$route.params;
    this.fetchRestaurant(id);
  },
  methods: {
    async fetchRestaurant(restaurantId) {
      try {
        // STEP 4: 透過 restaurantsAPI 取得餐廳資訊
        const { data } = await restaurantsAPI.getRestaurant({ restaurantId });

        // STEP 5: 透過 restaurantsAPI 取得餐廳資訊
        const { restaurant, isFavorited, isLiked } = data;
        const {
          id,
          name,
          Category,
          image,
          opening_hours: openingHours,
          tel,
          address,
          description,
          Comments,
        } = restaurant;

        this.restaurant = {
          id,
          name,
          categoryName: Category ? Category.name : "未分類",
          image,
          openingHours,
          tel,
          address,
          description,
          isFavorited,
          isLiked,
        };

        this.restaurantComments = Comments;
      } catch (error) {
        // STEP 6: 透過 restaurantsAPI 取得餐廳資訊
        Toast.fire({
          icon: "error",
          title: "無法取得餐廳資料，請稍後再試",
        });
      }
    },
    // async afterDeleteComment({ commentId }) {
    //   try {
    //     const { data } = await userAPI.deleteComment({
    //       commentId,
    //     });
    //     if (data.status !== "success") {
    //       throw new Error(data.message);
    //     }
    //     console.log(commentId);
    //     this.restaurantComments = this.restaurantComments.filter(
    //       (comment) => comment.id !== commentId
    //     );
    //   } catch (error) {
    //     console.error(error.message);
    //   }
    // },
    // afterDeleteComment(commentId) {
    //   // 以 filter 保留未被選擇的 comment.id
    //   this.restaurantComments = this.restaurantComments.filter(
    //     this.restaurantComments = this.restaurantComments.filter(
    //   );
    // },
    // afterCreateComment(payload) {
    //   const { commentId, restaurantId, text } = payload;
    //   this.restaurantComments.push({
    //     id: commentId,
    //     RestaurantId: restaurantId,
    //     User: {
    //       id: this.currentUser.id,
    //       name: this.currentUser.name,
    //     },
    //     text,
    //     createdAt: new Date(),
    //   });
    // },
  },
  beforeRouteUpdate(to, from, next) {
    const { id: restaurantId } = to.params;
    this.fetchRestaurant(restaurantId);
    next();
  },
};
</script>
