<template>
  <div>
    <h2 class="my-4">所有評論：</h2>

    <div v-for="comment in restaurantComments" :key="comment.id">
      <blockquote class="blockquote mb-0">
        <button
          type="button"
          class="btn btn-danger float-right"
          v-if="currentUser.isAdmin"
          @click.stop.prevent="handleDeleteButtonClick(comment.id)"
        >
          Delete
        </button>
        <h3>
          <a href="#"> {{ comment.User.name }} </a>
        </h3>
        <p>{{ comment.text }}</p>
        <footer class="blockquote-footer">
          {{ comment.createdAt | fromNow }}
        </footer>
      </blockquote>
      <hr />
    </div>
  </div>
</template>

<script>
import { fromNowFilter } from "./../utils/mixins";
import { mapState } from "vuex";
import userAPI from "./../apis/users";

// const dummyUser = {
//   currentUser: {
//     id: 1,
//     name: "管理者",
//     email: "root@example.com",
//     image: "https://i.pravatar.cc/300",
//     isAdmin: true,
//   },
//   isAuthenticated: true,
// };

export default {
  mixins: [fromNowFilter],
  props: {
    restaurantComments: {
      type: Array,
      required: true,
    },
  },
  computed: {
    ...mapState(["currentUser", "isAuthenticated"]),
  },
  methods: {
    // handleDeleteButtonClick(commentId) {
    //   console.log("handleDeleteButtonClick", commentId);
    //   // TODO: 請求 API 伺服器刪除 id 為 commentId 的評論
    //   // 觸發父層事件 - $emit( '事件名稱' , 傳遞的資料 )
    //   this.$emit("after-delete-comment", commentId);
    // },
    async handleDeleteButtonClick(commentId) {
      try {
        const { data } = await userAPI.deleteComment({
          commentId,
        });

        if (data.status !== "success") {
          throw new Error(data.message);
        }
        // this.restaurantComments = this.restaurantComments.filter(
        //   (this.restaurantComments = this.restaurantComments.filter())
        // );
      } catch (error) {
        console.error(error.message);
      }
    },
  },
};
</script>
