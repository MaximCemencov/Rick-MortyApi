<template>
  <div class="filter-container">
    <div>
      <input type="radio" id="filter-none" value="none" v-model="filterType">
      <label for="filter-none">No Filter</label>
    </div>
    <div>
      <input type="radio" id="filter-name" value="name" v-model="filterType">
      <label for="filter-name">Name</label>
      <div v-if="filterType === 'name'" class="filter-input">
        <input v-model="filterValue" placeholder="Enter name"/>
        <button @click="fetchFilteredCharacters">Filter</button>
      </div>
    </div>
    <div>
      <input type="radio" id="filter-status" value="status" v-model="filterType">
      <label for="filter-status">Status</label>
      <div v-if="filterType === 'status'" class="filter-input">
        <div class="filter-status-selector">
          <label>
            <input type="radio" value="alive" v-model="filterValue"> Alive
          </label>
          <label>
            <input type="radio" value="dead" v-model="filterValue"> Dead
          </label>
          <label>
            <input type="radio" value="unknown" v-model="filterValue"> Unknown
          </label>
        </div>
        <button @click="fetchFilteredCharacters">Filter</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {defineComponent, ref} from 'vue';

export default defineComponent({
  name: 'Filter',
  emits: ['filter'],
  setup(_, {emit}) {
    const filterType = ref<string>('none');
    const filterValue = ref<string>('');

    const fetchFilteredCharacters = () => {
      emit('filter', {type: filterType.value, value: filterValue.value});
    };

    return {
      filterType,
      filterValue,
      fetchFilteredCharacters,
    };
  },
});
</script>

<style scoped>
.filter-container {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  width: 100%;
  max-width: 310px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.filter-container > div {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.filter-container label {
  margin-bottom: 8px;
}

.filter-input {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.filter-status-selector {
  display: flex;
  flex-direction: column;
  align-items: start;
}

.filter-input input[type="text"],
.filter-input button {
  margin-top: 10px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.filter-input button {
  cursor: pointer;
  background-color: #4caf50;
  color: white;
}

.filter-input button:hover {
  background-color: #45a049;
}
</style>
