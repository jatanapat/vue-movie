<template>
  <div class="box">
    <h3 class="title">App: {{ movieId }}</h3>
    <h3 class="subtitle has-text-centered">Count: {{ status.count }}, Price: {{status.price}}</h3>
    <movie @chooseMovie="handleChooseMovie" :movieId="movieId"/>
    <seat
      :movieId="movieId"
      @chooseSeat="handleChooseSeat"
      :selectSeats="selectSeats"
    />
  </div>
</template>

<script>
import Movie from './components/Movie.vue';
import Seat from './components/Seat.vue';

export default {
  components: {
    Movie, Seat,
  },
  data() {
    return {
      movieId: '',
      selectSeats: [],
      status: { count: 0, price: 0 },
    };
  },
  methods: {
    handleChooseMovie(movieId) {
      if (this.status.count) {
        if (confirm('Data will be lost?')) {
          this.status = { count: 0, price: 0 };
          this.selectSeats = [];
        } else {
          return false;
        }
      }
      this.movieId = movieId;
    },
    handleChooseSeat(seat) {
      const ids = this.selectSeats.map(s => s.id);
      const idx = ids.indexOf(seat.id);

      if (idx === -1) {
        this.selectSeats.push(seat);
      } else {
        this.selectSeats.splice(idx, 1);
      }

      this.status = this.selectSeats.reduce((acc, s)=> {
        acc.count++;
        acc.price += s.price;
        return acc;
      }, { count: 0, price: 0 });
    }
  },
};
</script>
