<template>
  <div v-if="loading" class="social-callback">Loading...</div>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
      source: this.$route.query.source,
      code: this.$route.query.code,
      state: this.$route.query.state
    };
  },
  mounted() {
    this.socialLogin();
  },
  methods: {
    socialLogin() {
      const data = {
        source: this.source,
        code: this.code,
        state: this.state
      };
      this.axios
        .post("/api/system/social/blog/callback/", data)
        .then(res => {
          if (res.data.code != 200) {
            this.$toast({ type: "error", message: res.data.message });
            return;
          }
          this.loading = false;
          this.username = "";
          this.password = "";
          this.$store.commit("login", res.data.data);
          this.$store.commit("closeModel");
          this.$toast({ type: "success", message: "登录成功" });
          this.$router.push("/");
        })
        .catch(() => {
          this.loading = false;
        });
    }
  }
};
</script>
