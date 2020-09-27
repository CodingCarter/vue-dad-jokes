<template>
  <h1>Search dad jokes</h1>
  <form @submit="updateJokesForm">
    <label>
      Query:
      <input type="text" v-model="jokeQuery" />
    </label>
    <input type="submit" value="Go" />
  </form>
  <h3 v-if="!jokes">Loading...</h3>
  <ol v-else-if="jokes.length">
    <li v-for="{ id, joke } in jokes" v-bind:key="id">
      {{ joke }}
    </li>
  </ol>
  <h3 v-else>No jokes found</h3>
  <p>
    Built by Carter Snook with Vue 3 using the
    <a href="https://icanhazdadjoke.com/api" target="_blank">
      I Can Haz Dad Joke API
    </a>
    with an MIT open-source license.
  </p>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const jokeQuery = ref("");
    const jokes = ref(null);

    const reqOptions = { headers: { accept: "application/json" } };

    async function updateJokes() {
      jokes.value = null;

      const res = await fetch(
        `https://icanhazdadjoke.com/search?term=${jokeQuery.value}&limit=10`,
        reqOptions
      );

      const data = await res.json();

      jokes.value = data.results;
    }

    updateJokes();

    async function updateJokesForm(e) {
      e.preventDefault();
      updateJokes();
    }

    return { jokes, jokeQuery, updateJokesForm };
  },
};
</script>

<style lang="scss">
body {
  width: 100%;
  height: 100vh;
  margin: 0;
}

html,
input {
  font-family: "Nunito";
}

#app {
  padding: 2.5rem 4rem;

  h1 {
    font-size: 3rem;
    margin: 0;
    margin-bottom: 1rem;
  }

  input {
    padding: 0.25rem 0.5rem;

    &:first-of-type {
      margin-left: 0.5rem;
      margin-right: 1rem;
    }
  }

  ol {
    padding-left: 2.5rem;

    li {
      padding: 0.5rem 0;
      font-size: 1.1rem;
    }
  }

  p {
    font-size: 0.9rem;
    margin-top: 2rem;
  }
}
</style>
