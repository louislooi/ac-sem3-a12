<template>
  <div class="container py-5">
    <form @submit.stop.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          id="name"
          v-model="profile.name"
          type="text"
          name="name"
          class="form-control"
          placeholder="Enter Name"
          required
        />
      </div>

      <div class="form-group">
        <label for="image">Image</label>
        <img
          v-if="profile.image"
          :src="profile.image"
          class="d-block img-thumbnail mb-3"
          width="200"
          height="200"
        />
        <input
          id="image"
          type="file"
          name="image"
          accept="image/*"
          class="form-control-file"
          @change="handleFileChange"
        />
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import { mapState } from "vuex";
import usersAPI from "./../apis/users";
import { Toast } from "./../utils/helpers";
// const dummyData = {
//   profile: {
//     id: 1,
//     name: "root",
//     image: "https://i.imgur.com/58ImzMM.png",
//   },
// };
export default {
  components: {},
  data() {
    return {
      profile: {
        id: -1,
        name: "",
        image: "",
      },
    };
  },
  computed: {
    ...mapState(["currentUser"]),
  },
  created() {
    this.fetchProfile();
  },
  watch: {
    currentUser() {
      this.fetchProfile();
    },
  },
  methods: {
    fetchProfile() {
      this.profile.id = this.currentUser.id;
      this.profile.image = this.currentUser.image;
      this.profile.name = this.currentUser.name;
      console.log(this.profile.id);
    },

    handleFileChange(e) {
      const { files } = e.target;

      if (files.length === 0) {
        // 使用者沒有選擇上傳的檔案
        this.profile.image = "";
      } else {
        // 否則產生預覽圖
        const imageURL = window.URL.createObjectURL(files[0]);
        this.profile.image = imageURL;
      }
    },
    async handleSubmit(e) {
      try {
        console.log(e);
        const form = e.target;
        const formData = new FormData(form);
        let { id } = this.$route.params;
        let response = await usersAPI.update({
          userId: id,
          formData,
        });
        console.log("response", response);
        this.$emit("after-submit", formData);
      } catch (error) {
        console.log("error", error);
        Toast.fire({
          icon: "error",
          title: "無法送出資料，請稍後再試",
        });
      }
    },
  },
};
</script>
