<template>
  <div>
    <h2>Posts</h2>

    <form>
      <input
        type="text"
        class="
          shadow
          appearance-none
          border
          rounded
          w-full
          my-1
          py-2
          px-3
          text-gray-700
          leading-tight
          focus:outline-none
          focus:shadow-outline
        "
        name="title"
        v-model="title"
        placeholder="Create new post title"
      />

      <input
        type="text"
        class="
          shadow
          appearance-none
          border
          rounded
          w-full
          my-1
          py-2
          px-3
          text-gray-700
          leading-tight
          focus:outline-none
          focus:shadow-outline
        "
        name="body"
        v-model="body"
        placeholder="Create new post body"
      />

      <div class="flex items-center justify-between">
        <button
          type="button"
          class="
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-bold
            py-2
            px-4
            rounded
            focus:outline-none
            focus:shadow-outline
          "
          v-on:click="submitForm()"
        >
          Submit
        </button>
      </div>
    </form>
    <br />
    <div>
      <div class="card" v-for="(post, index) in posts" :key="index">
        <router-link :to="{ name: 'posts-id', params: { id: post.id } }">
          {{ post.title }}
        </router-link>

        <button v-on:click="editPost(post)">[Edit]</button>
        <button v-on:click="deletePost(post)">[Delete]</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: "layout",
  setup() {},
  methods: {
    async submitForm() {
      if (this.title == "" || this.body == "") {
        alert("Form is not valid ");
        return;
      }
      const ans = confirm("Save data ? ");
      if (ans) {
        let res;
        if (this.id == null) {
          const data = {
            title: this.title,
            body: this.body,
            userId: this.userId,
          };
          res = await this.$axios.$post(
            "https://jsonplaceholder.typicode.com/posts",
            data
          );
          this.posts.push(res);
        } else {
          const data = {
            title: this.title,
            body: this.body,
            userId: this.userId,
          };
          res = await this.$axios.$put(
            `https://jsonplaceholder.typicode.com/posts/${this.id}`,
            data
          );
          const { title, body } = res;
          const filtered = this.posts.find((p) => p.id === this.id);
          filtered.title = title;
          filtered.body = body;
        }

        this.title = "";
        this.body = "";
      }
    },
    editPost(post) {
      const { id, title, body } = post;
      this.title = title;
      this.body = body;
      this.id = id;
    },
    async deletePost(post) {
      if (post) {
        const ans = confirm("Are you sure ?");
        if (ans) {
          const { id } = post;
          const data = { id: id }
          await this.$axios.$delete(
            `https://jsonplaceholder.typicode.com/posts/${id}`, { data }
          );

          this.posts = this.posts.filter(x => x.id !== id)
        }
      }
    },
  },
  data: () => ({
    posts: [],
    title: "",
    body: "",
    userId: 1,
    id: null,
  }),
  async fetch() {
    this.posts = await this.$axios.$get(
      "https://jsonplaceholder.typicode.com/posts"
    );
  },
};
</script>

<style scoped>
.card {
  margin: 5px 2px;
  border: #777 1px solid;
  padding: 10px;
  cursor: pointer;
  border-radius: 2px;
}

.card:hover {
  background: #eee;
}
</style>