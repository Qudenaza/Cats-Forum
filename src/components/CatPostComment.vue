<template>
  <div class="post__comment comment">
    <div class="comment__wrapper">
      <div class="comment__image">
        <img src="http://placekitten.com/40/100" alt="Аватар коментатора" />
      </div>
      <div class="comment__data">
        <h3 class="post__title comment__title">
          {{comment.name}}
          <span>промурчал</span>
        </h3>
        <p class="post__text">{{comment.body}}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["comment"]
};
</script>
<style lang="scss">
.comment {
  padding: 0 15px;

  background: #fff;

  &__wrapper {
    display: flex;
    padding: 10px 15px;

    background: #e6f5ff;
    border: 1px solid #d6e5ef;
  }

  &__image {
    & img {
      width: 40px;
      height: 80px;
    }
  }

  &__data {
    margin-left: 15px;
  }

  &__title {
    margin-bottom: 5px;

    & span {
      font-size: 12px;
      color: #9d9e9f;
    }
  }
}
</style>