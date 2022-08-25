<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="this.showDetails = !this.showDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'editProject', params: { id: project.id }}">
          <span class="material-symbols-outlined icon">edit</span>
        </router-link>
        <span @click="deleteProject" class="material-symbols-outlined icon">delete</span>
        <span @click="toogleComplete" class="material-symbols-outlined icon tick">done</span>
      </div>
    </div>
    <div v-show="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "SingleProject",
  props: ['project'],
  data() {
    return {
      showDetails: false,
      url: 'http://localhost:3000/projects/' + this.project.id
    }
  },
  methods: {
    deleteProject() {
      fetch(this.url, { method: 'DELETE'})
          .then(() => this.$emit('delete', this.project.id))
          .catch(err => console.log(err.message))
    },
    toogleComplete() {
      fetch(this.url, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json'},
        body: JSON.stringify({ complete: !this.project.complete })
      }).then(() => {
        this.$emit('complete', this.project.id)
      }).catch(err => console.log(err))
    }
  }
}
</script>

<style scoped>
.project {
  margin: 20px auto;
  background: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
  border-left: 4px solid #e90074;
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.icon {
  font-size: 24px;
  margin-left: 10px;
  color: #777;
  cursor: pointer;
}

.icon:hover {
  color: #bbb;
}

.complete {
  border-left: 4px solid #00ce89;
}

.complete .tick {
  color: #00ce89;
}
</style>