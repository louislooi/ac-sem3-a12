<template>
  <div class="container py-5">
    <!-- User Profile Card -->
    <UserProfileCard
      :current-user="currentUser"
      :initial-profile="profile"
      :isFollowed="isFollowed"
    />

    <div class="row">
      <div class="col-md-4">
        <!-- UserFollowingsCard -->
        <UserFollowingsCard :initial-profile="profile" />
        <!-- UserFollowersCard -->
        <UserFollowersCard :initial-profile="profile" />
      </div>
      <div class="col-md-8">
        <!-- UserCommentsCard -->
        <UserCommentsCard :initial-profile="profile" />
        <!-- UserFavoritedRestaurantsCard -->
        <UserFavoritedRestaurantsCard :initial-profile="profile" />
      </div>
    </div>
  </div>
</template>

<script>
import UserProfileCard from "./../components/UserProfileCard";
import UserFollowingsCard from "./../components/UserFollowingsCard";
import UserFollowersCard from "./../components/UserFollowersCard";
import UserCommentsCard from "./../components/UserCommentsCard";
import UserFavoritedRestaurantsCard from "./../components/UserFavoritedRestaurantsCard";
import usersAPI from "./../apis/users";
import { mapState } from "vuex";
export default {
  components: {
    UserProfileCard,
    UserFollowingsCard,
    UserFollowersCard,
    UserCommentsCard,
    UserFavoritedRestaurantsCard,
  },
  data() {
    return {
      profile: {},
      isFollowed: "",
    };
  },
  computed: {
    ...mapState(["currentUser"]),
  },
  created() {
    const { id } = this.$route.params;
    this.fetchProfile(id);
  },
  beforeRouteUpdate(to, from, next) {
    const { id: userId } = to.params;
    this.fatchProfile(userId);
    next();
  },
  methods: {
    async fetchProfile(userId) {
      try {
        const { data } = await usersAPI.get({ userId });

        console.log(data);
        const { profile, isFollowed } = data;
        this.profile = profile;
        this.isFollowed = isFollowed;
      } catch (error) {
        console.error(error.message);
      }
    },
    // fetchProfile() {
    //   const { profile, isFollowed } = dummyData;
    //   this.profile = profile;
    //   this.isFollowed = isFollowed;
    // },
  },
};
</script>
