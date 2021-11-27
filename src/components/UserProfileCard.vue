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
          <div v-if="currentUser.id === initialProfile.id">
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
              @click.stop.prevent="deleteFollowing(profile.id)"
            >
              移除最愛
            </button>
            <button
              type="button"
              class="btn btn-primary btn-border favorite mr-2"
              v-else
              @click.stop.prevent="addFollowing(profile.id)"
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
import usersAPI from "./../apis/users";
import { Toast } from "./../utils/helpers";
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
    currentUser: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isFollowed: this.initialIsFollowed,
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
    initialIsFollowed(newValue) {
      this.initialIsFollowed = { ...newValue };
    },
  },
  methods: {
    async addFollowing(profileId) {
      try {
        // STEP 3: 使用撰寫好的 addFavorite 方法去呼叫 API，並取得回傳內容
        const { data } = await usersAPI.addFollowing({ userId: profileId });
        console.log(profileId);
        // STEP 4: 若請求過程有錯，則進到錯誤處理
        if (data.status !== "success") {
          throw new Error(data.message);
        }

        // STEP 5: 請求成功的話，改變 Vue 內的資料狀態
        this.isFollowed = true;
        this.initialIsFollowed = true;
      } catch (error) {
        // STEP 6: 請求失敗的話則跳出錯誤提示

        Toast.fire({
          icon: "error",
          title: "無法將餐廳加入最愛，請稍後再試",
        });
        console.log("error", error);
      }
    },
    async deleteFollowing(userId) {
      try {
        const { data } = await usersAPI.deleteFollowing({ userId });

        if (data.status !== "success") {
          throw new Error(data.message);
        }
        this.isFollowed = false;
        // this.profile = { ...this.profile, isFollowed: false };
      } catch (error) {
        Toast.fire({
          icon: "error",
          title: "無法將餐廳移除最愛，請稍後再試",
        });
        console.log("error", error);
      }
    },
  },
};
</script>
