<template>
  <section class="post">
    <div class="post__wrapper">
      <div class="post__image">
        <img src="https://placekitten.com/g/100/100" alt="Аватар пользователя" />
      </div>
      <div class="post__author">
        <h2 class="post__name">{{post.name}}</h2>
        <p class="post__title">{{post.title}}</p>
      </div>
      <p class="post__text">{{post.body}}</p>
      <footer class="post__footer">
        <button class="post__button" @click="showComments" v-if="showOpenButton">Открыть комментарии</button>
      </footer>
    </div>
    <div class="post__comments" v-if="showCommentsSection">
      <preloader v-if="!comments.length" />
      <cat-post-comment v-for="comment in comments" :key="comment.id" :comment="comment" />
      <button
        class="post__button post__button--close"
        @click="hideComments"
        v-show="comments.length"
      >Скрыть комментарии</button>
    </div>
  </section>
</template>

<script>
import CatPostComment from "./CatPostComment.vue";
import Preloader from "./Preloader.vue";

export default {
  data() {
    return {
      comments: [],
      showCommentsSection: false,
      showOpenButton: true
    };
  },
  methods: {
    getComments() {
      fetch(
        `https://jsonplaceholder.typicode.com/comments?postId=${this.post.id}`
      )
        .then(response => {
          if (response.ok) {
            return response.json();
          }

          throw new Error("Не получилось получить пользователей");
        })
        .then(json => {
          this.comments = json;
        })
        .catch(error => {
          console.log(error);
        });
    },
    showComments() {
      if (!this.comments.length) this.getComments();

      this.showCommentsSection = true;
      this.showOpenButton = false;
    },
    hideComments() {
      this.showCommentsSection = false;
      this.showOpenButton = true;
    }
  },
  props: ["post"],
  components: {
    CatPostComment,
    Preloader
  }
};
</script>
<style lang="scss">
.post {
  overflow: hidden;
  &:not(:last-child) {
    margin-bottom: 15px;
  }

  &__wrapper {
    position: relative;
    display: grid;
    width: 100%;
    padding: 10px 15px;
    z-index: 2;
    grid-gap: 10px;
    grid-template-columns: 100px auto;
    grid-template-areas:
      "image  author"
      "text text"
      "footer  footer";

    border: 1px solid #d6e5ef;
    background: #e6f5ff;

    @media (min-width: 720px) {
      grid-template-areas:
        "image  author"
        "image text"
        "footer  footer";
    }
  }

  &__comments {
    position: relative;
    z-index: 0;
    margin-bottom: 80px;
  }

  &__image {
    grid-area: image;
  }

  &__author {
    grid-area: author;
  }

  &__footer {
    grid-area: footer;
    text-align: right;
  }

  &__name {
    margin-bottom: 5px;

    font-size: 13px;
    font-weight: 400;
    color: #9d9e9f;
  }

  &__title {
    font-size: 15px;
    font-weight: 500;
  }

  &__text {
    grid-area: text;

    font-size: 13px;
  }

  &__button {
    background: transparent;
    color: #2892ff;
    border: none;
    transition: color 0.3s ease;

    &--close {
      position: absolute;
      padding: 5px;
      right: 15px;
      bottom: -25px;

      background: #e6f5ff;
    }

    &:hover {
      cursor: pointer;
      color: darken(#2892ff, 30%);
    }

    &:active,
    &:focus {
      outline: none;
      color: darken(#2892ff, 60%);
    }
  }
}
</style>