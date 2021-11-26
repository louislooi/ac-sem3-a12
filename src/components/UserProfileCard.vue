<template>
  <div class="card mb-3">
    <div class="row no-gutters">
      <div class="col-md-4">
        <img :src="profile.image" width="300px" height="300px" />
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">{{ profile.name }}</h5>
          <p class="card-text">{{ profile.email }}</p>
          <ul class="list-unstyled list-inline">
            <li>
              <strong>{{ profile.Comments.length }}</strong> 已評論餐廳
            </li>
            <li>
              <strong>{{ profile.FavoritedRestaurants.length }}</strong>
              收藏的餐廳
            </li>
            <li>
              <strong>{{ profile.Followings.length }}</strong> followings
              (追蹤者)
            </li>
            <li>
              <strong>{{ profile.Followers.length }}</strong> followers (追隨者)
            </li>
          </ul>
          <div v-if="profile.isAdmin">
            <router-link
              :to="{
                name: 'users-profile-edit',
                params: { id: profile.id },
              }"
              ><button type="submit" class="btn btn-primary">
                edit
              </button></router-link
            >
          </div>
          <div v-else>
            <button
              type="button"
              class="btn btn-danger btn-border favorite mr-2"
              v-if="isFollowed"
            >
              移除最愛
            </button>
            <button
              type="button"
              class="btn btn-primary btn-border favorite mr-2"
              v-else
            >
              加到最愛
            </button>
          </div>
        </div>
      </div>
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
    initialIsFollowed: {
      type: Boolean,
      require: true,
    },
  },
  data() {
    return {
      profile: this.initialProfile,
      isFollowed: this.initialIsFollowed,
    };
  },
  methods: {
    addFollowing() {
      this.profile = { ...this.profile, isFollowed: true };
    },
    deleteFollowing() {
      this.profile = { ...this.profile, isFollowed: false };
    },
  },
};
</script>
