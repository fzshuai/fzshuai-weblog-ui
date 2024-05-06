<template>
  <div v-if="loading" class="social-callback">Loading...</div>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
      code: this.$route.query.code,
      state: this.$route.query.state,
      source: this.$route.query.source
    };
  },
  mounted() {
    this.socialLogin();
  },
  methods: {
    socialLogin() {
      this.axios
        .get("/api/system/social/blog-social-callback/" + this.source, {
          params: {
            source: this.source,
            code: this.code,
            state: this.state
          }
        })
        .then(res => {
          if (res.data.code != 200) {
            this.$toast({ type: "error", message: res.data.message });
            return;
          }
          console.log(res.data.data);
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
