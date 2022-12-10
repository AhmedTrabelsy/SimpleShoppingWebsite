<template>
  <div class="sign-up container d-flex justify-content-between">
    <div class="child container p-5 d-flex flex-column justify-content-center">
      <form>
        <label for="name">Name:</label>
        <input type="text" name="name" v-model="name" required />
        <label for="Rating">Rating:</label>
        <textarea type="" name="Rating" v-model="rating" required></textarea>
        <button class="btn btn-primary d-flex m-auto" @click.prevent="addRating">Send<i class="bi bi-send"></i></button>
      </form>
    </div>
    <div class="ratings d-flex flex-column overflow-auto">
      <h3 class="display-5 ms-5">Ratings</h3>
      <div v-for="(user, index) in users" :key="index" class="card bg-light p-3 m-3">
        <h3>{{ user.name }}</h3>
        <hr />
        <p>{{ user.rating }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      rating: "",
      users: [
        {
          name: "Ahmed Trabelsi",
          rating:
            "Lorem ipsum dolor sit amet consectetur adipisicing elit. Placeat iste omnis minima eligendi exercitationem reprehenderit et nam molestias dolorum corporis asperiores, ex similique repellat praesentium quis inventore debitis quasi repellendus.",
        },
      ],
    };
  },
  methods: {
    addRating() {
      if (this.name && this.rating) {
        this.users.push({ name: this.name, rating: this.rating });
        this.name = "";
        this.rating = "";
      }
    },
    remove(e) {
      console.log(e);
      e.target.remove();
    },
  },
};
</script>

<style>
.signUp {
  background-color: #eee;
}
.ratings {
  max-height: 45vh;
}
</style>
