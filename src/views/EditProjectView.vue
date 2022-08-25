<template>
  <form @submit.prevent="handleSubmitEdit">
    <label>Title:</label>
    <input v-model="title" type="text" required>
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  name: "EditProjectView",
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      url: 'http://localhost:3000/projects/' + this.id
    }
  },
  mounted() {
    fetch(this.url)
        .then(res => res.json())
        .then(data => {
          this.title = data.title
          this.details = data.details
        })
  },
  methods: {
    handleSubmitEdit() {
      let projectEdit = {
        title: this.title,
        details: this.details,
      }

      fetch(this.url,{
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(projectEdit)
      }).then(() => {
        this.$router.push('/')
      }).catch(err => console.log(err))
    }
  }
}
</script>
