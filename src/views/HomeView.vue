<template>
  <div class="home">
    <FilterNav @filterChange="updateFilter" :current="current" />
    <div v-if="projects.length">
      <h1>Projects</h1>

      <div v-for="project in projects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "@/components/SingleProject.vue";
import FilterNav from "@/components/FilterNav.vue";

export default {
  name: "HomeView",

  components: { SingleProject, FilterNav },

  data() {
    return {
      projects: [],
      current: "all",
    };
  },

  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        this.projects = data;
      })
      .catch((err) => console.error("Error:", err));
  },

  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },

    handleComplete(id) {
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },

    updateFilter(by) {
      this.current = by;
      if (by === "all") {
        fetch("http://localhost:3000/projects")
          .then((res) => res.json())
          .then((data) => (this.projects = data));
      } else if (by === "completed") {
        fetch(`http://localhost:3000/projects?complete=1`)
          .then((res) => res.json())
          .then((data) => (this.projects = data));
      } else {
        fetch(`http://localhost:3000/projects?complete=0`)
          .then((res) => res.json())
          .then((data) => (this.projects = data));
      }
    },
  },
};
</script>

<style scoped>
</style>
