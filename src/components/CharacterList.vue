<template>
  <div class="box">
    <Filter @filter="applyFilter" />
    <div class="character-list">
      <CharacterItem v-for="character in characters" :key="character.id" :character="character" />
    </div>
    <Pagination
        v-if="info"
        :info="info"
        :currentPage="currentPage"
        @prevPage="prevPage"
        @nextPage="nextPage"
    />
  </div>
</template>

<script lang="ts">
import axios from 'axios';
import { defineComponent, ref, onMounted, watch } from 'vue';
import { Character, Info } from '@/types';
import CharacterItem from './CharacterItem.vue';
import Filter from '@/components/Filter.vue';
import Pagination from '@/components/Pagination.vue';

export default defineComponent({
  name: 'CharacterList',
  components: {
    Filter,
    CharacterItem,
    Pagination,
  },
  setup() {
    const characters = ref<Character[]>([]);
    const info = ref<Info | null>(null);
    const filterType = ref<string>('none');
    const filterValue = ref<string>('');
    const currentPage = ref<number>(1);
    const baseUrl = 'https://rickandmortyapi.com/api/character';

    const fetchData = async (url: string) => {
      try {
        const response = await axios.get(url);
        characters.value = response.data.results;
        info.value = response.data.info;
        const pageMatch = url.match(/page=(\d+)/);
        currentPage.value = pageMatch ? parseInt(pageMatch[1]) : 1;
      } catch (error) {
        console.error(error);
      }
    };

    const buildUrl = () => {
      let url = `${baseUrl}?page=${currentPage.value}`;
      if (filterType.value !== 'none' && filterValue.value) {
        url += `&${filterType.value}=${filterValue.value}`;
      }
      return url;
    };

    const fetchFilteredCharacters = () => {
      fetchData(buildUrl());
    };

    const applyFilter = ({ type, value }: { type: string; value: string }) => {
      filterType.value = type;
      filterValue.value = value;
      currentPage.value = 1;
      fetchFilteredCharacters();
    };

    const prevPage = () => {
      if (info.value && info.value.prev) {
        fetchData(info.value.prev);
      }
    };

    const nextPage = () => {
      if (info.value && info.value.next) {
        fetchData(info.value.next);
      }
    };

    onMounted(() => {
      fetchData(buildUrl());
    });

    watch([filterType, filterValue, currentPage], () => {
      fetchFilteredCharacters();
    });

    return {
      characters,
      info,
      filterType,
      filterValue,
      fetchFilteredCharacters,
      prevPage,
      nextPage,
      currentPage,
      applyFilter,
    };
  },
});
</script>

<style scoped>
.box {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.character-list {
  display: flex;
  flex-wrap: wrap;
  max-width: 1600px;
  justify-content: space-around;
}
</style>