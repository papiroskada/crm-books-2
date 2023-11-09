<template>
  <div>
    <table>
      <!-- Table headers -->
      <thead>
        <tr>
          <th>Checkbox</th>
          <th>Name</th>
          <th>Surname</th>
          <th>Middlename</th>
          <th>Age</th>
          <th>Gender</th>
          <th>Role</th>
        </tr>
      </thead>
      <!-- Table body -->
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td><input type="checkbox" v-model="user.selected" /></td>
          <td>{{ user.name }}</td>
          <td>{{ user.surname }}</td>
          <td>{{ user.middleName }}</td>
          <td>{{ calculateAge(user.birthDate) }}</td>
          <td>{{ user.gender }}</td>
          <td>{{ user.role }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <!-- Table code remains the same -->
      <button @click="deleteUser">Видалити користувача</button>
      <button @click="duplicateUser">Продублювати користувача</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["users"],
  methods: {
    calculateAge(birthDate) {
      const today = new Date();
      const birth = new Date(birthDate);
      let age = today.getFullYear() - birth.getFullYear();
      const monthDiff = today.getMonth() - birth.getMonth();

      if (
        monthDiff < 0 ||
        (monthDiff === 0 && today.getDate() < birth.getDate())
      ) {
        age--;
      }

      return age;
    },
    deleteUser() {
      console.log("deelete");
      const selectedUserIds = this.users
        .filter((user) => user.selected)
        .map((user) => user.id);
      this.$emit("deleteUser", selectedUserIds); // Corrected: "deleteUser" instead of "deleteUser"
    },
    duplicateUser() {
      console.log("duplicate");
      const selectedUserIds = this.users
        .filter((user) => user.selected)
        .map((user) => user.id);
      this.$emit("duplicateUser", selectedUserIds);
    },
  },
};
</script>

<style scoped>
/* Add your styling here */
</style>
