<template>
  <div class="page">
    <p>
      Oops, không thể tìm thấy trang. Trở về 
      <router-link to="/">Trở về trang chủ</router-link>
    </p>
  </div>
</template>

<script>
export default {
  name: 'NotFoundPage'
}
</script>

<style scoped>
.page {
  padding: 20px;
  text-align: center;
}
</style>
