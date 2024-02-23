<template>
  <div class="filter-nav">
    <button @click="updateFilter('all')" :class="{ active: current === 'all' }">
      View All
    </button>

    <button
      @click="updateFilter('completed')"
      :class="{ active: current === 'completed' }"
    >
      Completed
    </button>

    <button
      @click="updateFilter('ongoing')"
      :class="{ active: current === 'ongoing' }"
    >
      Ongoing
    </button>
  </div>
</template>

<script>
export default {
  name: "FilterNav",
  props: ["current"],
  methods: {
    updateFilter(by) {
      this.$emit("filterChange", by);
    },
  },
};
</script>

<style scoped>
.filter-nav button {
  background: none;
  border: none;
  color: #bbb;
  outline: none;
  font-size: 12px;
  text-transform: uppercase;
  margin-right: 10px;
  letter-spacing: 1px;
  font-weight: bold;
  cursor: pointer;
}

button.active {
  color: #000;
  border-bottom: 2px solid #000;
}
</style>
