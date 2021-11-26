<template>
  <div class="container py-5">
    <!-- AdminNav Component -->
    <AdminNav />
    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">Email</th>
          <th scope="col">Role</th>
          <th scope="col" width="140">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <th scope="row">{{ user.id }}</th>
          <td>{{ user.email }}</td>
          <td>{{ user.isAdmin ? "admin" : "user" }}</td>
          <td>
            <button
              type="button"
              class="btn btn-link"
              v-if="user.isAdmin"
              @click.stop.prevent="setUserAdmin(user.id)"
            >
              set as user
            </button>
            <button
              type="button"
              class="btn btn-link"
              v-else
              @click.stop.prevent="setUserAdmin(user.id)"
            >
              set as admin
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import AdminNav from "./../components/AdminNav";

const dummyData = {
  users: [
    {
      id: 1,
      name: "root",
      email: "root@example.com",
      password: "$2a$10$CEiIaYyaDuxsQWl1MULL7.2qX4QT7vhtIhuNtjsm.Rt0ajYRDBMa.",
      isAdmin: true,
      image: null,
      createdAt: "2021-11-09T12:50:29.000Z",
      updatedAt: "2021-11-09T12:50:29.000Z",
    },
    {
      id: 2,
      name: "user1",
      email: "user1@example.com",
      password: "$2a$10$OtQw6A2LBl8jByPWrok2C.TROAr9sOD.PCkb17WcaeaSbRWxHtDJ6",
      isAdmin: false,
      image: null,
      createdAt: "2021-11-09T12:50:29.000Z",
      updatedAt: "2021-11-09T12:50:29.000Z",
    },
    {
      id: 3,
      name: "user2",
      email: "user2@example.com",
      password: "$2a$10$31FAeCAKvDPx2Ob43tpDWeCAPNrxvwUdWlPNBvb.5cAGGzfNVAzhm",
      isAdmin: false,
      image: null,
      createdAt: "2021-11-09T12:50:29.000Z",
      updatedAt: "2021-11-09T12:50:29.000Z",
    },
  ],
};

export default {
  components: {
    AdminNav,
  },
  data() {
    return {
      users: {},
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      this.users = dummyData.users;
    },

    setUserAdmin(userId) {
      this.users = this.users.map((user) => {
        if (user.id === userId) {
          return {
            ...user,
            isAdmin: !user.isAdmin,
          };
        }
        return user;
      });
    },
  },
};
</script>
