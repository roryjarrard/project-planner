<template>
  <div class="home">
    <div v-if="projects.length">
      <h1>Projects</h1>

      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "@/components/SingleProject.vue";

export default {
  name: "HomeView",

  components: { SingleProject },

  data() {
    return {
      projects: [],
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
  },
};
</script>
