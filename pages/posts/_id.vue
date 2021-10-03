<template>
  <div>
    <header>
      <h5>Post detail</h5>

      <div class="card">
        <h4>TITLE : {{ post.title }}</h4>
        <br>
        <p class="card-text">
          {{ post.body }}
        </p>
      </div>
    </header>
  </div>
</template>

<script>
export default {
 layout: "layout",
  setup() {},
  data: () => ({
    post: {}
  }),
  async fetch() {
    const id = this.$route.params.id;
    this.post = await this.$axios.$get(`https://jsonplaceholder.typicode.com/posts/${id}`);
  },
}
</script>

<style>

</style>