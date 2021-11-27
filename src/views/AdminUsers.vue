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
              @click.stop.prevent="toggleUserRole(user.id, user.isAdmin)"
            >
              set as user
            </button>
            <button
              type="button"
              class="btn btn-link"
              v-else
              @click.stop.prevent="toggleUserRole(user.id, user.isAdmin)"
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
import usersAPI from "./../apis/users";
import adminAPI from "./../apis/admin";
import { Toast } from "./../utils/helpers";
import { mapState } from "vuex";
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
  computed: {
    ...mapState(["currentUser"]),
  },
  methods: {
    async fetchUsers() {
      try {
        const { data } = await usersAPI.getUsers();
        const { users } = data;
        this.users = users;
      } catch (error) {
        console.error(error.message);
      }
    },
    async toggleUserRole(userId, isAdmin) {
      try {
        console.log(userId, isAdmin);
        const willBeAdmin = !isAdmin;
        const { data, statusText } = await adminAPI.users.update({
          userId,
          isAdmin: willBeAdmin.toString(),
        });

        console.log(this.userId, this.isAdmin);
        if (statusText !== "OK" || data.status !== "success") {
          throw new Error(statusText);
        }
        this.users = this.users.map((user) => {
          if (user.id !== userId) {
            return user;
          }
          return {
            ...user,
            isAdmin: willBeAdmin,
          };
        });
      } catch (error) {
        Toast.fire({
          type: "error",
          title: "無法更新會員角色，請稍後再試",
        });
      }
    },
    // async setUserAdmin(userId) {
    //   try {
    //     const { data } = await adminAPI.users.update({ userId });
    //     console.log(data);
    //     this.users = this.users.map((user) => {
    //       if (user.id === userId) {
    //         return {
    //           ...user,
    //           isAdmin: !user.isAdmin,
    //         };
    //       }
    //       return user;
    //     });
    //   } catch (error) {
    //     Toast.fire({
    //       type: "error",
    //       title: "無法更新會員角色，請稍後再試",
    //     });
    //   }
    //   // setUserAdmin(userId) {
    //   //   this.users = this.users.map((user) => {
    //   //     if (user.id === userId) {
    //   //       return {
    //   //         ...user,
    //   //         isAdmin: !user.isAdmin,
    //   //       };
    //   //     }
    //   //     return user;
    //   //   });
    //   // },
    // },
  },
};
</script>
