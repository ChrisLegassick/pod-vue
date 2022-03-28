<template>
  <form>
    <label>Select a breed:</label>
    <select v-model="selectedBreed">
      <option v-for="breed in breeds" :key="breed">{{ breed }}</option>
    </select>
    <button @click="handleSelect">Fetch!</button>
  </form>
</template>

<script>
export default {
  name: "Search",
  data() {
    return {
      breeds: [],
      selectedBreed: "affenpinscher"
    };
  },
  async created() {
    const data = await fetch("https://dog.ceo/api/breeds/list");
    const breeds = await data.json();
    this.breeds = await breeds.message;
  },
  methods: {
    handleSelect(e) {
      e.preventDefault();
      this.$emit("handle-select", this.selectedBreed);
    }
  }
};
</script>

<style scoped>
button {
  padding: 0.8em 1em;
  text-transform: uppercase;
  border-radius: 0.5rem;
}
</style>