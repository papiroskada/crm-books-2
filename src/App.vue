<template>
  <div id="app">
    <RegForm @submitForm="addUser" />
    <UsersTable
      :users="users"
      @deleteUser="deleteUser"
      @duplicateUser="duplicateUser"
    />
  </div>
</template>

<script>
import RegForm from "./components/RegForm.vue";
import UsersTable from "./components/UsersTable.vue";

export default {
  name: "App",
  components: {
    RegForm,
    UsersTable,
  },
  data() {
    return {
      users: [],
    };
  },
  methods: {
    addUser(user) {
      user.selected = false;
      console.log(user);
      this.users.push(user);
    },
    deleteUser(selectedUserIds) {
      this.users = this.users.filter(
        (user) => !selectedUserIds.includes(user.id)
      );
    },
    generateUniqueId() {
      // Assuming a simple approach using a timestamp
      return "id_" + new Date().getTime();
    },
    duplicateUser(selectedUserIds) {
      const duplicatedUsers = this.users.filter((user) =>
        selectedUserIds.includes(user.id)
      );

      duplicatedUsers.forEach((user) => {
        // Set selected to false for duplicated users
        user.selected = false;

        // Create a deep copy of the user to avoid references
        const duplicatedUser = { ...user, id: this.generateUniqueId() };
        this.users.push(duplicatedUser);
      });
    },
  },
};
</script>

<style>
#app {
  background-color: #c1fff0;
}
</style>
