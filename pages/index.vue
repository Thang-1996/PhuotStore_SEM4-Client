<template>
  <div>
    <h1>Home</h1>
  </div>
</template>

<script>
export default {
  created() {
    const token = this.$auth.$storage.getUniversal('token')
    if (!token) {
      this.$router.push('/login')
    }
  },
}
</script>
