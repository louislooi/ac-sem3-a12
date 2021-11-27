<template>
  <div class="container py-5">
    <div>
      <h1>{{ restaurant.name }}</h1>
      <span class="badge badge-secondary mt-1 mb-3">
        {{ restaurant.categoryName }}
      </span>
    </div>

    <hr />

    <ul>
      <li>評論數： {{ restaurant.commentsLength }}</li>
      <li>瀏覽次數： {{ restaurant.viewCounts }}</li>
    </ul>

    <button type="button" class="btn btn-link" @click="$router.back()">
      回上一頁
    </button>
  </div>
</template>

<script>
import restaurantsAPI from "./../apis/restaurants";
// const dummyData = {
//   restaurant: {
//     id: 1,
//     name: "Mauricio Runolfsdottir MD",
//     tel: "(099) 532-4016",
//     address: "4551 Rowe Mills",
//     opening_hours: "08:00",
//     description:
//       "Fugit temporibus modi magni ut.\nEnim omnis blanditiis dolores saepe.\nIn omnis doloribus et odit quaerat.",
//     image:
//       "https://loremflickr.com/320/240/restaurant,food/?random=3.477217787673492",
//     viewCounts: 0,
//     createdAt: "2021-11-09T12:50:29.000Z",
//     updatedAt: "2021-11-09T12:50:29.000Z",
//     CategoryId: 3,
//     Category: {
//       id: 3,
//       name: "義大利料理",
//       createdAt: "2021-11-09T12:50:29.000Z",
//       updatedAt: "2021-11-09T12:50:29.000Z",
//     },
//     Comments: [
//       {
//         id: 101,
//         text: "Praesentium voluptates autem amet in laudantium aut rem unde.",
//         UserId: 1,
//         RestaurantId: 1,
//         createdAt: "2021-11-09T12:50:29.000Z",
//         updatedAt: "2021-11-09T12:50:29.000Z",
//         User: {
//           id: 1,
//           name: "root",
//           email: "root@example.com",
//           password:
//             "$2a$10$CEiIaYyaDuxsQWl1MULL7.2qX4QT7vhtIhuNtjsm.Rt0ajYRDBMa.",
//           isAdmin: true,
//           image: null,
//           createdAt: "2021-11-09T12:50:29.000Z",
//           updatedAt: "2021-11-09T12:50:29.000Z",
//         },
//       },
//       {
//         id: 51,
//         text: "Velit alias maxime.",
//         UserId: 2,
//         RestaurantId: 1,
//         createdAt: "2021-11-09T12:50:29.000Z",
//         updatedAt: "2021-11-09T12:50:29.000Z",
//         User: {
//           id: 2,
//           name: "user1",
//           email: "user1@example.com",
//           password:
//             "$2a$10$OtQw6A2LBl8jByPWrok2C.TROAr9sOD.PCkb17WcaeaSbRWxHtDJ6",
//           isAdmin: false,
//           image: null,
//           createdAt: "2021-11-09T12:50:29.000Z",
//           updatedAt: "2021-11-09T12:50:29.000Z",
//         },
//       },
//       {
//         id: 1,
//         text: "Omnis ut velit.",
//         UserId: 3,
//         RestaurantId: 1,
//         createdAt: "2021-11-09T12:50:29.000Z",
//         updatedAt: "2021-11-09T12:50:29.000Z",
//         User: {
//           id: 3,
//           name: "user2",
//           email: "user2@example.com",
//           password:
//             "$2a$10$31FAeCAKvDPx2Ob43tpDWeCAPNrxvwUdWlPNBvb.5cAGGzfNVAzhm",
//           isAdmin: false,
//           image: null,
//           createdAt: "2021-11-09T12:50:29.000Z",
//           updatedAt: "2021-11-09T12:50:29.000Z",
//         },
//       },
//     ],
//   },
// };

export default {
  data() {
    return {
      restaurant: {
        id: -1,
        name: "",
        categoryName: "",
        commentsLength: 0,
        viewCounts: 0,
      },
    };
  },

  created() {
    console.log(this.$route.params);
    const { id } = this.$route.params;

    this.fetchRestaurant(id);
  },
  beforeRouteUpdate(to, from, next) {
    const { id: restaurantId } = to.params;
    this.fetchRestaurant(restaurantId);
    next();
  },
  methods: {
    async fetchRestaurant(restaurantId) {
      try {
        const { data } = await restaurantsAPI.getRestaurant({ restaurantId });
        const { id, name, Category, Comments, viewCounts } = data.restaurant;
        this.restaurant = {
          ...this.restaurant,
          id,
          name,
          categoryName: Category ? Category.name : "未分類",
          commentsLength: Comments.length,
          viewCounts,
        };
      } catch (error) {
        console.error(error.message);
      }
    },
    // fetchRestaurant() {
    //   const { id, name, Category, Comments, viewCounts } = dummyData.restaurant;
    //   this.restaurant = {
    //     ...this.restaurant,
    //     id,
    //     name,
    //     categoryName: Category ? Category.name : "未分類",
    //     commentsLength: Comments.length,
    //     viewCounts,
    //   };
    // },
  },
};
</script>
