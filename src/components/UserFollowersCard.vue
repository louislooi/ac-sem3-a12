<template>
  <div class="card mt-4">
    <div class="card-header">
      <strong>{{ profile.Followers.length }}</strong> followers (追隨者)
    </div>
    <div class="card-body">
      <a class="m-1" v-for="followers in profile.Followers" :key="followers.id">
        <img :src="followers.image" width="60" height="60" class="avatar" />
      </a>
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
  watch: {
    initialProfile(newValue) {
      this.profile = {
        ...this.profile,
        ...newValue,
      };
    },
  },
};
</script>
