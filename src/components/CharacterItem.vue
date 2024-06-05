<template>
  <div class="character-card">
    <img :src="character.image" :alt="character.name" class="character-image" />
    <div class="character-info">
      <h2>{{ character.name }}</h2>
      <p class="status">
        <span :class="statusClass(character.status)"></span>
        {{ character.status }} - {{ character.species }}
      </p>
      <p class="location">
        <strong>Last known location:</strong>
        <br />
        {{ character.location.name }}
      </p>
      <p class="first-seen">
        <strong>First seen in:</strong>
        <br />
        {{ character.origin.name }}
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Character } from '@/types/Character';

export default defineComponent({
  name: 'CharacterItem',
  props: {
    character: {
      type: Object as PropType<Character>,
      required: true
    }
  },
  methods: {
    statusClass(status: string) {
      return {
        'status-indicator': true,
        'status-dead': status === 'Dead',
        'status-alive': status === 'Alive',
        'status-unknown': status === 'unknown',
      };
    },
  },
});
</script>

<style scoped>
.character-card {
  display: flex;
  background-color: #2e2f32;
  color: white;
  margin: 10px;
  border-radius: 10px;
  width: 45%;
  max-height: 225px;
  min-width: 350px;
  max-width: 500px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
}

.character-image {
  width: 45%;
  max-width: 225px;
  min-width: 150px;
  object-fit: fill;
  aspect-ratio: 1;
  border-radius: 10px;
  margin-right: 10px;
}

.character-info {
  text-align: start;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  height: 100%;
}

.character-info h2 {
  margin: 0;
  font-size: 24px;
}

.status {
  display: flex;
  align-items: center;
  margin: 0;
}

.status-indicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 10px;
}

.status-dead {
  background-color: red;
}

.status-alive {
  background-color: green;
}

.status-unknown {
  background-color: gray;
}


.location, .first-seen {
  color: #a9a9a9;
  margin: 0;
}

.location strong, .first-seen strong {
  color: #ffffff;
}
</style>
