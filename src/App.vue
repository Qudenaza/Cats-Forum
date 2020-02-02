<template>
  <div class="container">
    <cat-post-filters v-on:parentEvent="onFilter" />
    <div class="posts">
      <cat-post v-for="post in filteredPosts" :key="post.id" :post="post" />
      <p class="posts__empty" v-show="!filteredPosts.length">Ничего не найдено</p>
    </div>
  </div>
</template>

<script>
import CatPost from "./components/CatPost.vue";
import CatPostFilters from "./components/CatPostFilters.vue";

export default {
  data() {
    return {
      filterName: "",
      filterText: "",
      posts: []
    };
  },
  methods: {
    onFilter(data) {
      this.filterName = data.name;
      this.filterText = data.text;
    }
  },
  created() {
    const self = this;

    function getUserNames(callback) {
      fetch("https://jsonplaceholder.typicode.com/users")
        .then(response => {
          if (response.ok) {
            return response.json();
          }

          throw new Error("Не получилось получить пользователей");
        })
        .then(users => {
          callback(users);
        })
        .catch(error => {
          console.log(error);
        });
    }

    function getPosts(users) {
      fetch("https://jsonplaceholder.typicode.com/posts")
        .then(response => {
          if (response.ok) {
            return response.json();
          }

          throw new Error("Не получилось получить посты");
        })
        .then(content => {
          let i = 0;

          content.forEach(post => {
            if (self.posts.length > 20) return;

            self.posts.push({
              id: post.id,
              name: users[i++].name,
              title: post.title,
              text: post.body
            });

            if (i >= 10) i = 0;
          });
        })
        .catch(error => {
          console.log(error);
        });
    }

    getUserNames(getPosts);
  },
  computed: {
    filteredPosts() {
      return this.posts.filter(post => {
        return (
          post.name.includes(this.filterName) &&
          post.text.includes(this.filterText)
        );
      });
    }
  },
  components: {
    CatPost,
    CatPostFilters
  }
};
</script>

<style lang="scss">
*,
*::before,
*::after {
  padding: 0;
  margin: 0;
  box-sizing: inherit;
}

body {
  box-sizing: border-box;
  font-family: "Open Sans", sans-serif;
}

.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 10px 15px;

  @media (min-width: 1440px) {
    display: flex;
    flex-direction: row-reverse;
  }
}

.posts {
  flex-grow: 1;

  &__empty {
    margin-top: 70px;

    text-align: center;
    color: #ccc;
  }
}
</style>
