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
  mounted() {
    const self = this;

    async function getData() {
      // Получаем имена пользователей
      const userNames = await fetch(
        "https://jsonplaceholder.typicode.com/users"
      ).then(res => res.json());

      // Получаем заголовк поста, и его содержание
      const userPosts = await fetch(
        "https://jsonplaceholder.typicode.com/posts?_limit=20"
      ).then(res => res.json());

      // Присваеваем данные полученные с сервера в наш массив posts
      self.posts = userPosts;

      // Вспомогательная переменная счетчик
      let i = 0;

      // В каждый из обьектов поста добавляем имя полученное с сервера
      self.posts.map(post => {
        post["name"] = userNames[i++].name;

        if (i >= 10) i = 0;
      });
    }

    getData();
  },
  computed: {
    filteredPosts() {
      return this.posts.filter(post => {
        return (
          post.name.includes(this.filterName) &&
          post.body.includes(this.filterText)
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
