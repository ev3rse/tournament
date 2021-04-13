<template>
  <div id="app">
    <div class="config">
      <strong>Bracket Size:</strong>
      <input type="text" v-model="size" placeholder="128, 64, 32, 16, 8, 4, 2">
      <button @click="generatePlayers" type="button">Сгенерировать игроков</button>
    </div>
    <div class="content">
      <draggable class="list drop" :list="players" group="players">
        <div class="list-item" v-for="(item, index) in players" :key="index">
          <span>{{ item.name }}</span>
        </div>
      </draggable>
      <BracketGenerator
          ref="refBracketGenerator"
          :bracket-size="parseInt(size)"
          :players="players"
          @updateBracketSize="updateSize"
      />
    </div>
  </div>
</template>

<script>
import BracketGenerator from "./components/BracketGenerator.vue";
import draggable from 'vuedraggable';

export default {
  name: 'App',
  components: { BracketGenerator, draggable },
  data () {
    return {
      size: 8,
      players: [],
    }
  },
  methods: {
    updateSize(size) {
      this.size = size
    },
    generatePlayers() {
      this.players = []
      this.$refs.refBracketGenerator.cleanMatch()

      for (let i = 0; i < this.size; i += 1) {
        this.players.push({
          id: i,
          name: 'player-' + (i + 1)
        })
      }
    },
  }
}
</script>

<style lang="scss">
*,
*::after,
*::before {
  box-sizing: border-box;
}

html,
body {
  font-family: Helvetica, Arial, sans-serif;
}

#app {
  background-color: #e9ecef;
  padding: 15px;
}

.config {
  padding-bottom: 15px;
  padding-left: 10px;
}

.config {
  input {
    padding: 5px 12px;
    margin-left: 12px;
  }

  button {
    margin-left: 12px;
  }
}

.content {
  display: flex;
}

.list {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px 0;
  margin: 10px 0;
}

.list-item {
  min-width: 200px;
  background-color: #ffffff;
  border: 1px solid rgba(0,0,0,.125);
  padding: 5px 20px;
  margin-right: 15px;
  margin-bottom: 8px;
  box-shadow: 0 0 2px 0 rgba(0,0,0,.1);

  &:hover {
    cursor: move;
  }
}
</style>
