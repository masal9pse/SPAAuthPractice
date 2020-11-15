<template>
  <section>
    <h1>管理画面へようこそ</h1>
    <!--  thisがいらない -->
    <button @click="$router.push('/admin/logout')">ログアウトページへ</button>
  </section>
</template>

<script>
export default {
  middleware: "not_logined_admin_user",
  layout: "admin"
};
</script>
