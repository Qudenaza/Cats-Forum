<template>
  <section class="filter">
    <h2 class="filter__title">Фильтры</h2>
    <form class="filter__form">
      <div class="filter__group">
        <label class="filter__label" for="post-name">Имя/название</label>
        <input
          class="filter__input"
          v-on:input="childEvent"
          type="text"
          id="post-name"
          v-model="postName"
        />
      </div>
      <div class="filter__group">
        <label class="filter__label" for="post-content">Контент</label>
        <input
          class="filter__input"
          v-on:input="childEvent"
          type="text"
          id="post-content"
          v-model="postContent"
        />
      </div>
    </form>
  </section>
</template>

<script>
export default {
  data() {
    return {
      postName: "",
      postContent: ""
    };
  },
  methods: {
    childEvent() {
      this.$emit("parentEvent", {
        name: this.postName,
        text: this.postContent
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.filter {
  width: 100%;
  max-width: 320px;
  margin: 0 auto 15px auto;
  padding: 15px;

  background: #cccccc;

  @media (min-width: 1440px) {
    margin-left: 15px;
  }

  &__title {
    font-size: 18px;
    font-weight: 400;
    margin-bottom: 10px;
  }
  &__group {
    &:not(:last-child) {
      margin-bottom: 5px;
    }
  }

  &__label {
    display: block;
    margin-left: 5px;
  }

  &__input {
    width: 250px;
    padding: 5px;
  }
}
</style>