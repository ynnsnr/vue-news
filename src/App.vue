<template>
  <v-app>

    <!-- Navbar -->
    <v-toolbar dark color="primary">
      <v-toolbar-title class="headline">
        <a @click="store.dispatch('reload')" class="white--text font-weight-light">News</a>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
        <a @click="store.dispatch('reload')" class="white--text font-weight-light">
          <v-icon>refresh</v-icon>
        </a>
      </v-btn>
    </v-toolbar>

    <v-content>

      <!-- Title & filter button -->
      <v-container pb-0>
        <div class="d-flex" style="align-items: center;">
          <div class="headline font-weight-light">Headlines</div>
          <div style="text-align: right;">
            <v-menu  max-height="50vh" offset-y left origin="top right" transition="scale-transition">
              <v-btn slot="activator" color="primary" dark>
                Filter
              </v-btn>
              <v-list>
                <v-list-tile
                  v-for="(source, i) in sources" :key="i"
                  @click="store.dispatch('filter', source.id)"
                >
                  <v-list-tile-title>{{ source.name }}</v-list-tile-title>
                </v-list-tile>
              </v-list>
            </v-menu>
          </div>
        </div>
      </v-container>

      <!-- Headlines grid -->
      <v-container grid-list-lg>
        <Headlines v-bind:headlines="headlines" />
      </v-container>

    </v-content>

    <!-- Footer -->
    <v-footer height="48" color="primary" class="pa-3 white--text">
      <v-spacer></v-spacer>
      <div>&copy; {{ new Date().getFullYear() }}</div>
    </v-footer>

  </v-app>
</template>

<script>
import Headlines from './components/Headlines';
import Vue from 'vue';
import Vuex from 'vuex';
// import 'es6-promise/auto'; // TODO: decide if we support IE8

// Variables
const API_KEY = '68152bd7d6764f1197d83cb4c5cef867';
const API_BASE_URL = 'https://newsapi.org/v2';

// API calls to https://newsapi.org
const fetchHeadlines = (url) => {
  return fetch(url)
    .then(response => response.json())
    .then(data => data.articles.filter(a => a.urlToImage && a.content));
}

const fetchSources = (url) => {
  return fetch(url)
    .then(response => response.json())
    .then(data => data.sources);
}

// Store
Vue.use(Vuex);
const store = new Vuex.Store({
  state: {
    headlines: [],
    sources: []
  },
  mutations: {
    setHeadlines (state, headlines) {
      state.headlines = headlines;
    },
    setSources (state, sources) {
      state.sources = sources;
    }
  },
  actions: {
    async filter ({ commit }, id) {
      const url = `${API_BASE_URL}/top-headlines?sources=${id}&apiKey=${API_KEY}`;
      store.commit('setHeadlines', await fetchHeadlines(url));
    },
    async reload ({ commit }) {
      const url = `${API_BASE_URL}/top-headlines?country=us&apiKey=${API_KEY}`;
      store.commit('setHeadlines', await fetchHeadlines(url));
    },
    async getSources ({ commit }) {
      const url = `${API_BASE_URL}/sources?apiKey=${API_KEY}`;
      store.commit('setSources', await fetchSources(url));
    }
  },
});

export default {
  name: 'App',
  components: {
    Headlines
  },
  beforeMount () {
    this.store.dispatch('reload');
    this.store.dispatch('getSources');
  },
  computed: {
    headlines: () => store.state.headlines,
    sources: () => store.state.sources,
    store: () => store
  }
}
</script>
