<template>
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
</template>

<script>
export default {
  props: {
    initialCurrentUser: {
      type: Object,
      default: () => ({
        id: -1,
        name: "",
        image: "",
      }),
    },
  },
  data() {
    return {
      profile: {
        id: -1,
        name: "",
        image: "",
      },
    };
  },
  created() {
    this.profile = { ...this.profile, ...this.initialCurrentUser };
    console.log(this.profile);
  },
  watch: {
    initialCurrentUser(newValue) {
      this.profile = {
        ...this.profile,
        ...newValue,
      };
    },
  },
  methods: {
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
    handleSubmit(e) {
      const form = e.target; // <form></form>
      const formData = new FormData(form);
      this.$emit("after-submit", formData);
    },
  },
};
</script>
