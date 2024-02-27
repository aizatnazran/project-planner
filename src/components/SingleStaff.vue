<template>
  <div class="staff">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ staff.name }}</h3>
      <p @click="showDetails = !showDetails">{{ staff.position }}</p>
      <div class="icons">
        <span @click="confirmDelete" class="material-icons">delete</span>
        <router-link :to="{ name: 'EditStaff', params: { id: staff.id } }">
          <span class="material-icons">edit</span>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  props: ["staff"],
  data() {
    return {
      showDetails: false,
      uri: "", // Initialize URI here
    };
  },
  mounted() {
    this.uri = "http://localhost:3000/staffs/" + this.staff.id;
  },
  methods: {
    confirmDelete() {
      Swal.fire({
        title: `Are you sure you want to delete ${this.staff.name}?`,
        icon: "warning",
        showCancelButton: true,
        confirmButtonText: "Yes, delete it!",
        cancelButtonText: "No, cancel!",
        reverseButtons: true,
      }).then((result) => {
        if (result.isConfirmed) {
          this.deleteStaff();
        }
      });
    },
    deleteStaff() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => {
          Swal.fire({
            icon: "success",
            title: "Staff Deleted Successfully!",
            showConfirmButton: false,
            timer: 1500,
          });
          this.$emit("delete", this.staff.id);
        })
        .catch((err) => console.log(err));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.staff.complete }),
      })
        .then(() => {
          this.$emit("complete", this.staff.id);
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style scoped>
.staff {
  margin: 20px auto;
  padding: 10px 20px;
  border-radius: 4px;
  background: white;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
  margin-left: 10px;
  color: #bbb;
  font-size: 24px;
  cursor: pointer;
}
.material-icons:hover {
  color: #777;
}
/* completed projects */
</style>
