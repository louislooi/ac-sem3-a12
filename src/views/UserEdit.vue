<template>
  <div class="container py-5">
    <UserEditForm
      :initial-profile="profile"
      @after-submit="handleAfterSubmit"
    />
  </div>
</template>

<script>
import UserEditForm from "./../components/UserEditForm";
const dummyData = {
  profile: {
    id: 1,
    name: "root",
    image: "https://i.imgur.com/58ImzMM.png",
  },
};
export default {
  components: {
    UserEditForm,
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
    const { id } = this.$route.params;
    this.fetchProfile(id);
  },
  methods: {
    fetchProfile() {
      this.profile = dummyData.profile;
    },
    handleAfterSubmit(formData) {
      // 透過 API 將表單資料送到伺服器
      for (let [name, value] of formData.entries()) {
        console.log(name + ": " + value);
      }
    },
  },
};
</script>
