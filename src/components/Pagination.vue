<template>
  <div class="pagination-container" v-if="info">
    <button @click="prevPage" :disabled="!info.prev">Previous</button>
    <span>Page {{ currentPage }} of {{ info.pages }}</span>
    <button @click="nextPage" :disabled="!info.next">Next</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Info } from '@/types';

export default defineComponent({
  name: 'Pagination',
  props: {
    info: {
      type: Object as () => Info,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  emits: ['prevPage', 'nextPage'],
  setup(props, { emit }) {
    const prevPage = () => {
      emit('prevPage');
    };

    const nextPage = () => {
      emit('nextPage');
    };

    return {
      prevPage,
      nextPage,
    };
  },
});
</script>

<style scoped>
.pagination-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.pagination-container button {
  margin: 0 10px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.pagination-container button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.pagination-container span {
  margin: 0 10px;
  font-size: 16px;
}
</style>
