<template>
  <form @submit.stop.prevent="handleSubmit">
    <div class="form-group mb-4">
      <label for="text">留下評論：</label>
      <textarea class="form-control" rows="3" name="text" v-model="text" />
    </div>
    <div class="d-flex align-items-center justify-content-between">
      <button type="button" class="btn btn-link" @click="$router.back()">
        回上一頁
      </button>
      <button type="submit" class="btn btn-primary mr-0">Submit</button>
    </div>
  </form>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
import userAPI from "./../apis/users";

export default {
  props: {
    restaurantId: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      text: "",
    };
  },
  methods: {
    async handleSubmit() {
      try {
        const { data } = await userAPI.addComment({
          commentId: uuidv4(),
          restaurantId: this.restaurantId,
          text: this.text,
        });
        if (data.status !== "success") {
          throw new Error(data.message);
        }
        this.text = "";
      } catch (error) {
        console.error(error.message);
      }
    },
  },
};
</script>
