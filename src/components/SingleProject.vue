<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="toggleDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-symbols-outlined"> edit </span>
        </router-link>
        <span @click="deleteProject" class="material-symbols-outlined">
          delete
        </span>
        <span @click="toggleComplete" class="material-symbols-outlined tick">
          done
        </span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "SingleProject",
  props: {
    project: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      showDetails: false,
      uri: `http://localhost:3000/projects/${this.project.id}`,
    };
  },

  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },

    deleteProject() {
      fetch(this.uri, { method: "DELETE" }).then(() =>
        this.$emit("delete", this.project.id)
      );
    },

    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => this.$emit("complete", this.project.id))
        .catch((err) => console.error("Error:", err));
    },
  },
};
</script>

<style scoped>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 4px solid #e90074;
}
.project.complete {
  border-left: 4px solid #00ce89;
}

.project.complete .tick {
  color: #00ce89;
}

h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-symbols-outlined {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}

.material-symbols-outlined:hover {
  color: #777;
}
</style>
