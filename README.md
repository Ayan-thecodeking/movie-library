# 🎬 TMDB Movies App

A **Vue 3 + Nuxt 3** application that lets users browse, search, and manage their favorite movies using the [TMDB API](https://developer.themoviedb.org/).  
Users can explore trending and “Now Playing” titles, search by multiple filters, and build a persistent personal **watchlist** stored in `localStorage`.

---

## 🧭 Project Goal

Build a movie-browsing web application where users can explore films, search by title, and manage a personal “watchlist” that persists between sessions.

---

## ✨ Core Features

- 🔍 **Search movies** by title, year, or genre  
- 🎞️ Browse **Now Playing** movies from TMDB  
- 📄 Detailed **Movie Info** view with ratings, synopsis, and release date  
- ❤️ Add or remove movies from a **Watchlist (Favorites)** — persisted with `localStorage`  
- 📱 Fully **responsive and mobile-friendly** design  
- 🔁 **Pagination** support for both “Now Playing” and search results  

---

## 💡 Bonus Features

- Client-side routing via **Nuxt 3**
- Centralized state management using **Pinia**
- API key stored securely with **environment variables**
- Clean, reusable Vue components for cards, pagination, and loaders

---

## 🧩 Evaluation Criteria

| Category | What It Demonstrates |
|-----------|----------------------|
| **Dev Skills & Code Quality** | Reusable Vue components, clean state handling, API integration, use of `.env` |
| **Completion** | Movie search, persistent watchlist, multiple views implemented |
| **UX** | Simple, responsive, intuitive interface |

---

## ⚙️ Tech Stack

- 🧱 [Nuxt 3](https://nuxt.com/) (Vue 3 + Vite)
- 🗂 [Pinia](https://pinia.vuejs.org/) for state management
- 🌐 [Axios](https://axios-http.com/) (via Nuxt `$request` plugin)
- 🎨 SCSS for modern styling
- 🎬 [TMDB API](https://developer.themoviedb.org/) for movie data

---

## 🛠️ Setup & Installation

### 1️⃣ Clone the repository
```bash
git clone https://github.com/Ayan-thecodeking/movie-library.git
cd movie-library
```

### 2️⃣ Install dependencies
Install all required packages for the project:

```bash
npm install

```
### 3️⃣ Add your TMDB API key

To connect your app with **The Movie Database (TMDB)** API, you’ll need an API key.

#### 🔑 Step 1: Get your TMDB API key
- Go to [https://www.themoviedb.org/settings/api](https://www.themoviedb.org/settings/api)
- Log in or create an account
- Generate a **Developer API Key**

#### 🧩 Step 2: Create a `.env` file
In the **root directory** of your project, create a file named `.env` and add your API key as follows:

```bash
# .env
NUXT_PUBLIC_TMDB_API_KEY=your_tmdb_api_key_here

```

### 4️⃣ Run the development server

After setting up your environment and dependencies, start the Nuxt 3 development server:

```bash
npm run dev

```
### 5️⃣ Build for Production

When your app is ready to go live, you can create an optimized production build.

#### ⚙️ Step 1: Build the app
Run the following command to generate a production-ready build:

```bash
npm run build
npm run start

```
### 🧪 Running Tests

You can set up **unit tests** for your Vue components using **Jest** and **Vue Test Utils**.

Testing ensures your core features — like searching movies, pagination, and favorites — work reliably even after code updates.

---

#### 1️⃣ Install test dependencies

Install Jest and Vue Test Utils as dev dependencies:

```bash
npm install --save-dev jest @vue/test-utils vue-jest
 ```

 ### 2️⃣ Configure Jest

Once Jest and Vue Test Utils are installed, you need to configure Jest to work properly with Vue 3 and Nuxt 3.

---

#### ⚙️ Step 1: Create a Jest configuration file

Create a file named `jest.config.js` in the root of your project:

```js
export default {
  testEnvironment: 'jsdom', // Simulate a browser environment for Vue components
  moduleFileExtensions: ['js', 'json', 'vue'],
  transform: {
    '^.+\\.vue$': 'vue-jest',  // Transforms Vue single-file components
    '^.+\\.js$': 'babel-jest', // Transforms ES6+ JavaScript
  },
  moduleNameMapper: {
    '^~/(.*)$': '<rootDir>/$1', // Support Nuxt-style aliases
  },
  transformIgnorePatterns: ['/node_modules/'],
}
```
### 3️⃣ Run Tests

Once your test setup is complete, you can easily run all unit tests in your project using **Jest**.

---

#### ▶️ Step 1: Run all tests

Use the following command to execute your test suite:

```bash
npm test
```

### 🧠 Project Structure

The project is organized with a clear and modular structure — optimized for scalability, readability, and easy navigation.

```bash
movie-library/
├── assets/                     # Static assets (images, icons, SCSS, etc.)
│   └── no_img.png              # Fallback image for movies without posters
│
├── components/                 # Reusable UI components
│   ├── MovieCard.vue           # Displays movie poster, title, and fav button
│   ├── Pagination.vue          # Handles page navigation (prev/next)
│   └── Loading.vue             # Shows loading spinner and messages
│
├── pages/                      # Nuxt 3 pages (auto-routed)
│   ├── index.vue               # Home page (Now Playing + Search)
│   ├── favorites.vue           # Favorites/Watchlist view
│   └── movie/
│       └── [id].vue            # Movie details page with extra info
│
├── stores/                     # Pinia state management
│   └── movies.js               # Handles movie fetching, favorites, pagination
│
├── plugins/                    # Custom Nuxt plugins
│   └── request.js              # Axios wrapper for API calls
│
├── public/                     # Public static files served as-is
│
├── tests/                      # Jest unit tests
│   └── MovieCard.spec.js       # Example test for MovieCard.vue
│
├── .env                        # Environment variables (TMDB API key)
├── .env.example                # Example environment file
├── .babelrc                    # Babel config for Jest
├── jest.config.js              # Jest setup file
├── nuxt.config.ts              # Nuxt configuration (SSR, plugins, runtime config)
├── package.json                # Dependencies and scripts
└── README.md                   # Project documentation
```