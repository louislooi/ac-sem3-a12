<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">美食達人</h1>
    <hr />
    <div class="row text-center">
      <UserGrid :initialUsers="user" v-for="user in users" :key="user.id" />
    </div>
  </div>
</template>

<script>
/* eslint-disable */

import NavTabs from "./../components/NavTabs";
import UserGrid from "./../components/UserGrid.vue";


const dummyData = {
  users: [
    {
      id: 1,
      name: "root",
      email: "root@example.com",
      password: "$2a$10$CEiIaYyaDuxsQWl1MULL7.2qX4QT7vhtIhuNtjsm.Rt0ajYRDBMa.",
      isAdmin: true,
      image:
        "https://i.picsum.photos/id/1003/1181/1772.jpg?hmac=oN9fHMXiqe9Zq2RM6XT-RVZkojgPnECWwyEF1RvvTZk",
      createdAt: "2021-11-09T12:50:29.000Z",
      updatedAt: "2021-11-09T12:50:29.000Z",
      Followers: [],
      FollowerCount: 0,
      isFollowed: true,
    },
    {
      id: 2,
      name: "user1",
      email: "user1@example.com",
      password: "$2a$10$OtQw6A2LBl8jByPWrok2C.TROAr9sOD.PCkb17WcaeaSbRWxHtDJ6",
      isAdmin: false,
      image:
        "https://i.picsum.photos/id/1012/3973/2639.jpg?hmac=s2eybz51lnKy2ZHkE2wsgc6S81fVD1W2NKYOSh8bzDc",
      createdAt: "2021-11-09T12:50:29.000Z",
      updatedAt: "2021-11-09T12:50:29.000Z",
      Followers: [],
      FollowerCount: 0,
      isFollowed: false,
    },
    {
      id: 3,
      name: "user2",
      email: "user2@example.com",
      password: "$2a$10$31FAeCAKvDPx2Ob43tpDWeCAPNrxvwUdWlPNBvb.5cAGGzfNVAzhm",
      isAdmin: false,
      image:
        "https://i.picsum.photos/id/1025/4951/3301.jpg?hmac=_aGh5AtoOChip_iaMo8ZvvytfEojcgqbCH7dzaz-H8Y",
      createdAt: "2021-11-09T12:50:29.000Z",
      updatedAt: "2021-11-09T12:50:29.000Z",
      Followers: [],
      FollowerCount: 0,
      isFollowed: false,
    },
  ],
};

export default {
  components: {
    NavTabs,
    UserGrid,
  },
  data() {
    return {
      users: [],
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      this.users = dummyData.users;
    },
  },
};
</script>
