<script setup>
import MovieForm from "@/MovieForm.vue";
import MovieItem from "@/MovieItem.vue";
import { items } from "./movies.json";
import {computed, defineAsyncComponent, ref} from "vue";
import AppModal from "@/components/AppModal.vue";

const movies = ref(items);
const currentMovie = ref();
function updateRating(id, rating) {
  movies.value = movies.value.map((movie) => {
    if (movie.id === id) {
      movie.rating = rating;
    }
    return movie;
  });
}
function removeMovie(id) {
  movies.value = movies.value.filter((movie) => movie.id !== id);
}
function editMovie(id) {
  currentMovie.value = movies.value.find((movie) => movie.id === id);
  showForm();
}
function saveMovie(data) {
  const isNew = !!movies.value.find((movie) => movie.id === data.id);
  if (!isNew) {
    addMovie(data);
  } else {
    updateMovie(data);
  }
}
function updateMovie(data) {
  movies.value = movies.value.map((m) => {
    if (m.id === data.id) {
      data.rating = m.rating;
      return data;
    }
    return m;
  });
  hideForm();
}
function addMovie(data) {
  movies.value.push(data);
  hideForm();
}
const showMovieForm = ref(false);
function hideForm() {
  showMovieForm.value = false;
  currentMovie.value = null;
}
function showForm() {
  showMovieForm.value = true;
}
const averageRating = computed(() => {
  const avg = movies.value
    .map((movie) => parseInt(movie.rating || 0))
    .reduce((a, b) => a + b, 0);
  return Number(avg / movies.value.length).toFixed(1);
});
const totalMovies = computed(() => {
  return movies.value.length;
});
function removeRatings() {
  movies.value = movies.value.map((movie) => {
    movie.rating = null;
    return movie;
  });
}
const AppButton = defineAsyncComponent({
  loader: ()=>import('./components/AppButton.vue'),
  delay: 200
});
</script>

<template>
  <div class="app">
    <AppModal v-if="showMovieForm" title="Movie Form" @close="hideForm">
      <template #body>
        <MovieForm
            @update:modelValue="saveMovie"
            :modelValue="currentMovie"
            @cancel="hideForm"
        />
      </template>
    </AppModal>
    <div class="movie-actions-list-wrapper">
      <div class="movie-actions-list-info">
        <span>Total Movies: {{ totalMovies }}</span>
        <span> / </span>
        <span>Average Rating: {{ averageRating }}</span>
      </div>
      <div class="flex-spacer"></div>
      <div class="movie-actions-list-actions">
        <AppButton
          @click="removeRatings"
          :disabled="false"
          type="button"
        >
          Remove Ratings
        </AppButton>

        <Suspense>
          <AppButton
              @click="removeRatings"
              :disabled="false"
              type="button"
          >
            Remove Ratings Suspense 🍟
          </AppButton>
          <!-- loading state via #fallback slot -->
          <template #fallback>
            Loading...
          </template>

        </Suspense>

        <button
          class="movie-actions-list-action-button"
          :class="{
            'button-primary': !showMovieForm,
            'button-disabled': showMovieForm,
          }"
          @click="showForm"
          :disabled="showMovieForm"
        >
          Add Movie
        </button>
      </div>
    </div>
    <div class="movie-list">
      <MovieItem
        v-for="movie in movies"
        :key="movie.id"
        :movie="movie"
        @edit="editMovie"
        @remove="removeMovie"
        @update:rating="updateRating"
      />
    </div>
  </div>
</template>
