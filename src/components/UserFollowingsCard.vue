<template>
  <div class="card">
    <div class="card-header">
      <strong>{{ profile.Followings.length }}</strong> followings (追蹤者)
    </div>
    <div class="card-body">
      <a
        class="m-1"
        v-for="followings in profile.Followings"
        :key="followings.id"
      >
        <img :src="followings.image" width="60" height="60" class="avatar" />
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
};
</script>
