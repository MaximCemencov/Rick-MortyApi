<template>
  <CharacterList />
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import CharacterList from './components/CharacterList.vue';

@Options({
  components: {
    CharacterList,
  },
})

export default class App extends Vue {}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
