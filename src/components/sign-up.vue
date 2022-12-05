<template>
  <div class="sign-up container w-100">
    <div class="child container p-5 d-flex flex-column justify-content-center">
      <form>
        <label for="name">Name</label>
        <input type="text" name="name" v-model="name" required />
        <label for="password">Password</label>
        <input type="password" v-model="password" required />
        <select v-model="selected">
          <option value="Developer">Developer</option>
          <option value="Dentiste">Dentiste</option>
          <option value="Plasticien">Plasticien</option>
          <option value="Roler">Roler</option>
        </select>
        <br>
        <label for="terms">Terms and Conditions</label>
        <input type="checkbox" name="terms" v-model="terms" required />
        <label for="names">Ahmed</label>
        <input type="checkbox" name="names" value="ahmed" v-model="names"/>
        <label for="names">Emna</label>
        <input type="checkbox" name="names" value="emna" v-model="names"/>
        <label for="names">Ghada</label>
        <input type="checkbox" name="names" value="ghada" v-model="names"/>
        <button type="submit">submit</button>
      </form>
    </div>
    <div
      class="child container p-5 d-flex flex-column justify-content-center text-center"
    >
      <h1>Name: {{ name }}</h1>
      <h1>Password: {{ password }}</h1>
      <h1>{{ selected }}</h1>
      <h1>{{ terms }}</h1>
      <h1 v-for="(name, index) in names" :key="index">{{name}}</h1>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      password: "",
      selected: "Developer",
      terms: "false",
      names: [],
    };
  },
};
</script>

<style>
.signUp {
  background-color: #eee;
}
.child {
  background-color: white;
}
</style>
