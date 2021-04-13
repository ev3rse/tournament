<template>
  <div class="bracket">
    <div v-for="(round, roundIndex) in rounds" :key="roundIndex" class="round">
      <Match
          ref="refMatch"
          v-for="(match, matchIndex) in groupTeam(generateRound(round))"
          :key="matchIndex"
          :is-draggable="roundIndex === 0"
          :match="roundIndex === 0 ? convertArrayToObject(match) : match"
      />
    </div>
  </div>
</template>

<script>
import Match from "./Match.vue";

export default {
  name: "BracketGenerator",
  components: { Match },
  props: {
    bracketSize: {
      type: Number,
      default: 8
    },
    players: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data() {
    return {
      defaultRounds: [256, 128, 64, 32, 16, 8, 4, 2, 1]
    }
  },
  computed: {
    rounds() {
      this.$emit('updateBracketSize', this.bracketSize)

      return this.defaultRounds.filter(rounds => rounds <= this.bracketSize)
    },
  },
  methods: {
    groupTeam(round) {
      if (round.length === 1) {
        return [round]
      }

      const groupedRound = []

      for (let i = 0; i < round.length; i += 2) {
        if (round[i] !== undefined && round[i + 1] !== undefined) {
          groupedRound.push([ round[i], round[i + 1] ])
        }
      }

      return groupedRound
    },
    generateRound(length) {
      return Array.from(Array(length)).map(() => Math.floor(Math.random() * (100 + 1)))
    },
    convertArrayToObject(array) {
      return array.map((item, index) => {
        return {
          id: index,
          name: ''
        }
      })
    },
    cleanMatch() {
      this.$refs.refMatch.forEach(item => item.clean())
    }
  }
}
</script>

<style lang="scss" scoped>
.bracket {
  display: flex;
  flex-grow: 1;
}

.round::v-deep {
  display: flex;
  flex-grow: 1;
  flex-direction: column;

  &:first-child {
    .team::before {
      display: none;
    }

    .team__content::before {
      display: none;
    }
  }

  &:last-child {
    .team::after {
      display: none;
    }
  }
}
</style>