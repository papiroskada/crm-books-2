<template>
  <div>
    <table class="table">
      <thead>
        <tr>
          <th>Вибір</th>
          <th>Ім'я</th>
          <th>Призвіще</th>
          <th>По батькові</th>
          <th>Вік</th>
          <th>Стать</th>
          <th>Роль</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td><input type="checkbox" v-model="user.selected" /></td>
          <td>{{ user.name }}</td>
          <td>{{ user.surname }}</td>
          <td>{{ user.middleName }}</td>
          <td>{{ calculateAge(user.birthDate) }}</td>
          <td>{{ replaceVariants(user.gender, "gender") }}</td>
          <td>{{ replaceVariants(user.role, "role") }}</td>
        </tr>
      </tbody>
    </table>
    <div class="text-center">
      <button class="btn btn-light m-1" @click="deleteUser">
        Видалити користувача
      </button>
      <button class="btn btn-light" @click="duplicateUser">
        Продублювати користувача
      </button>
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
      const selectedUserIds = this.users
        .filter((user) => user.selected)
        .map((user) => user.id);
      this.$emit("deleteUser", selectedUserIds);
    },
    duplicateUser() {
      const selectedUserIds = this.users
        .filter((user) => user.selected)
        .map((user) => user.id);
      this.$emit("duplicateUser", selectedUserIds);
    },
    replaceVariants(value, type) {
      const genderVariants = {
        male: "Чоловіча",
        female: "Жіноча",
      };

      const roleVariants = {
        1: "Адміністратор",
        2: "Користувач",
        3: "Автор",
      };

      if (type === "gender") {
        return genderVariants[value] || value;
      } else if (type === "role") {
        return roleVariants[value] || value;
      } else {
        return value;
      }
    },
  },
};
</script>

<style scoped></style>
