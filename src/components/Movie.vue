<template>
    <div class="box">
        <h3 class="title">{{ movieId }}</h3>
        <div class="columns">
            <template v-for="m in movies">
                <div :key="m.id" :class="className(m.id)" @click="chooseMovie(m.id)">
                    <figure class="image">
                        <img :src="imgSrc(m.id)">
                    </figure>
                </div>
            </template>
        </div>
    </div>
</template>

<script>
import { movies } from '../others/movie.json';

export default {
  props: ['movieId'],
  data() {
    return {
      movies,
    };
  },
  methods: {
    imgSrc(movieId) {
      return `/movies/${movieId}.jpg`;
    },
    chooseMovie(movieId) {
      this.$emit('chooseMovie', movieId);
    },
    className(movieId) {
      const classStyle = ['column', 'pointer'];
      if (this.movieId === movieId) {
        classStyle.push('chosen');
      }
      return classStyle;
    },
  },
  mounted() {
    this.chooseMovie(movies[0].id);
  },
};
</script>

<style>
.pointer {
    cursor: pointer;
}

.chosen {
    border-style: solid;
}
</style>
