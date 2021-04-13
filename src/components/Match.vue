<template>
  <div class="match">
    <div v-if="isDraggable" class="match__content">
      <template v-if="match.length > 1">
        <div class="team">
          <draggable class="team__content drop" :list="list1" group="players">
            <span v-for="(item, index) in list1" :key="index">{{ item.name }}</span>
          </draggable>
        </div>
        <div class="team">
          <draggable class="team__content drop" :list="list2" group="players">
            <span v-for="(item, index) in list2" :key="index">{{ item.name }}</span>
          </draggable>
        </div>
      </template>
      <template v-else>
        <div class="team drop">
          <draggable class="team__content drop" :list="list3" group="players">
            <span v-for="(item, index) in list3" :key="index">{{ item.name }}</span>
          </draggable>
        </div>
      </template>
    </div>

    <div v-else class="match__content">
      <div class="team" v-for="(team, teamIndex) in match" :key="teamIndex">
        <div class="team__content"></div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  name: "Match",
  components: { draggable },
  props: {
    match: {
      type: Array,
      default: () => {
        return []
      }
    },
    isDraggable: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      list1: [],
      list2: [],
      list3: [],
    }
  },
  methods: {
    clean() {
      this.list1 = []
      this.list2 = []
      this.list3 = []
    }
  }
}
</script>

<style lang="scss" scoped>

.match {
  display: flex;
  flex-grow: 1;
  flex-direction: column;

  &__content {
    display: flex;
    flex-grow: 1;
    flex-direction: column;
  }
}

.team {
  display: flex;
  flex-grow: 1;
  flex-direction: column;
  justify-content: center;
  position: relative;
  margin: 0 10px;
  padding: 6px 0;
  
  &:hover {
    cursor: move;
  }

  &::before {
    min-height: 30px;
    content: '';
    display: block;
    position: absolute;
    left: -10px;
    top: 50%;
    border-left: 2px solid purple;
    margin-top: -15px;
    margin-left: -2px;
  }

  &:nth-child(odd) {
    &::after {
      width: 10px;
      height: 50%;
      content: "";
      display: block;
      position: absolute;
      right: -10px;
      top: 50%;
      border: 2px solid transparent;
      border-top-color: purple;
      border-right-color: purple;
    }
  }

  &:nth-child(even) {
    &::after {
      width: 10px;
      height: 50%;
      content: "";
      display: block;
      position: absolute;
      right: -10px;
      bottom: 50%;
      border: 2px solid transparent;
      border-bottom-color: purple;
      border-right-color: purple;
    }
  }

  &__content {
    width: 100%;
    height: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid green;
    position: relative;
    user-select: none;
  
    &::before {
      width: 10px;
      content: "";
      display: block;
      position: absolute;
      top: 50%;
      left: -10px;
      border-bottom: 2px solid purple;
      margin-left: -2px;
    }
  }
}
</style>