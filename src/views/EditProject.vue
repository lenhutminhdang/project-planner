<template>
  <form @submit.prevent="handleSubmit">
    <label>Title</label>
    <input type="text" v-model="title" required />
    <label>Details</label>
    <textarea v-model="details" required></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      uri: "http://localhost:3000/projects/" + this.id,
      title: "",
      details: "",
    };
  },
  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((project) => {
        (this.title = project.title), (this.details = project.details);
      })
      .catch((err) => console.log(err));
  },
  methods: {
    handleSubmit() {
      const editedProject = {
        title: this.title,
        details: this.details,
      };

      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify(editedProject),
      })
        .then(() => this.$router.push({ name: "Home" }))
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style></style>
