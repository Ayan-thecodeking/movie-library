<template>
  <div class="favorites">
    <!-- Header -->
    <div class="header-row">
      <button class="back-btn" @click="router.back()" aria-label="Go back">← Back</button>
      <h1>
        Your Favorites
        <span v-if="favMovies.length">({{ favMovies.length }})</span>
      </h1>
    </div>

    <!-- No favorites message -->
    <p v-if="!favMovies.length" class="no-favs">
      You haven't added any favorites yet ❤️  
      <br />
      Go find a movie you love and tap the <strong>♡</strong> icon to add it!
    </p>

    <!-- Favorites grid -->
    <div v-else class="movies-grid compact">
      <div
        v-for="m in favMovies"
        :key="m.id"
        class="fav-item"
      >
        <MovieCard
          :movie="m"
          :is-favorite="store.isFavorite(m.id)"
          @toggle-fav="store.toggleFavorite(m.id)"
        />
        <button class="remove-btn" @click="store.toggleFavorite(m.id)">
          Remove from Favorites
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { useMoviesStore } from '~/stores/movies'
import MovieCard from '~/components/MovieCard.vue'

const router = useRouter()
const store = useMoviesStore()

/**
 * The key improvement here:
 * We ensure that even after a refresh, favorites are restored from localStorage
 * and we fetch the actual movie details for each ID if not already loaded.
 */
const favMovies = ref([])

// Load favorites & ensure movies are available
onMounted(async () => {
  store._loadFavorites()

  // If we already have movies in memory (from now playing/search), reuse them
  const combined = [...store.movies, ...store.searchedMovies]
  const existingFavs = combined.filter((m) => store.isFavorite(m.id))

  // If no movie data exists (fresh reload), fetch details for each ID
  if (!existingFavs.length && store.favorites.length) {
    const { public: { tmdbApiKey } } = useRuntimeConfig()
    const { $request, $Keys } = useNuxtApp()

    const fetched = []
    for (const id of store.favorites) {
      try {
        const res = await $request($Keys.GET, `https://api.themoviedb.org/3/movie/${id}`, {
          params: { api_key: tmdbApiKey, language: 'en-US' },
          isTokenRequired: false,
        })
        fetched.push(res.data)
      } catch (e) {
        console.warn('Failed to load favorite movie', id)
      }
    }
    favMovies.value = fetched
  } else {
    favMovies.value = existingFavs
  }
})
</script>

<style scoped lang="scss">
.favorites {
  padding: 24px 16px;
  color: #fff;
}

.header-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 16px;

  h1 {
    margin: 0;
    font-size: 22px;
    color: #fff;
  }

  .back-btn {
    background: #2b2b2b;
    color: #fff;
    border: none;
    border-radius: 6px;
    padding: 8px 12px;
    font-size: 14px;
    cursor: pointer;
    transition: background 0.2s;
  }

  .back-btn:hover {
    background: #3a3a3a;
  }
}

/* No favorites */
.no-favs {
  color: #bbb;
  text-align: center;
  padding: 40px 0;
  font-size: 16px;
}

/* Grid Layout */
.movies-grid {
  display: grid;
  gap: 24px;
  grid-template-columns: repeat(1, minmax(0, 1fr));

  @media (min-width: 600px) {
    grid-template-columns: repeat(2, 1fr);
  }

  @media (min-width: 900px) {
    grid-template-columns: repeat(3, 1fr);
  }

  @media (min-width: 1200px) {
    grid-template-columns: repeat(4, 1fr);
  }
}

/* Compact style for cards */
.movies-grid.compact .fav-item {
  transform: scale(0.95);
  transform-origin: top left;
}

/* Remove Button */
.remove-btn {
  margin-top: 8px;
  width: 100%;
  padding: 8px 12px;
  border-radius: 6px;
  border: 1px solid #444;
  background: #2d2d2d;
  color: #fff;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s;
}

.remove-btn:hover {
  background: #3a3a3a;
}
</style>
