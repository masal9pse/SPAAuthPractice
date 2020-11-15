<template>
  <section>
    <h1>logout</h1>
    <form @submit.prevent="logout">
      <button type="submit">logoutボタン</button>
    </form>
  </section>
</template>

<script>
export default {
  middleware: "not_logined_admin_user",
  methods: {
    async logout() {
      await this.$store.dispatch("auth/logout");
      this.$router.push("./login");
    }
  }
};
</script>
