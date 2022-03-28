<template>
  <main class="container">
    <Modal v-if="modalOpen" :image="selectedImage" @close-modal="closeModal" />
    <h1>Plenty Of Dogs!</h1>
    <div class="btn-container">
      <button @click="searchDoggos">Search</button>
      <button @click="fetchRandomDoggos">Random</button>
    </div>
    <Search @handle-select="fetchDoggos" v-if="state === 'search' && doggos.length === 0" />
    <div class="grid">
      <Image v-for="doggo in doggos" :key="doggo" :doggo="doggo" @click="openModal" />
    </div>
  </main>
</template>

<script>
import Modal from "./components/Modal.vue";
import Image from "./components/Image.vue";
import Search from "./components/Search.vue";

export default {
  name: "App",
  components: {
    Modal,
    Image,
    Search
  },
  data() {
    return {
      state: "search",
      doggos: [],
      modalOpen: false,
      selectedImage: ""
    };
  },
  methods: {
    async fetchDoggos(selectedBreed) {
      const data = await fetch(
        `https://dog.ceo/api/breed/${selectedBreed}/images/random/5`
      );
      const fetchedDoggos = await data.json();
      fetchedDoggos.message.map(doggo => {
        this.doggos = [...this.doggos, doggo];
      });

      this.getNextDoggos(selectedBreed);
    },

    searchDoggos() {
      this.state = "search";
      this.doggos = [];
    },

    async fetchRandomDoggos() {
      if (this.state === "search") {
        this.doggos = [];
        this.state = "random";
      }

      const data = await fetch("https://dog.ceo/api/breeds/image/random/5");
      const fetchedDoggos = await data.json();
      fetchedDoggos.message.map(doggo => {
        this.doggos = [...this.doggos, doggo];
      });
      this.getNextDoggos();
    },

    openModal(e) {
      this.modalOpen = true;
      this.selectedImage = e.target.src;
    },

    closeModal() {
      this.modalOpen = false;
    },

    getNextDoggos(selectedBreed) {
      window.onscroll = () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          if (this.state === "search") {
            this.fetchDoggos(selectedBreed);
          } else if (this.state === "random") {
            this.fetchRandomDoggos();
          }
        }
      };
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #222;
  margin-top: 60px;
}

.btn-container {
  display: flex;
  justify-content: space-around;
  width: 70%;
  margin: auto;
}

.btn-container > button {
  border: none;
  background: none;
  font-size: 1.5rem;
  cursor: pointer;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1rem;
}
</style>
