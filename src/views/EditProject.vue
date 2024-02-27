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
import Swal from "sweetalert2"; // Import SweetAlert2

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
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      })
      .catch((err) => console.log(err));
  },
  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title: this.title, details: this.details }),
      })
        .then(() => {
          Swal.fire({
            title: "Success!",
            text: "Project successfully edited",
            icon: "success",
            confirmButtonText: "OK",
          }).then(() => {
            this.$router.push("/");
          });
        })
        .catch((err) => {
          console.log(err);
          Swal.fire({
            title: "Error!",
            text: "Failed to edit project",
            icon: "error",
            confirmButtonText: "OK",
          });
        });
    },
  },
};
</script>


<style></style>
