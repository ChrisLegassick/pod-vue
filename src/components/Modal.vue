<template>
  <div class="modal-backdrop" @click="closeModal">
    <img :src="image" alt="Randomly generated doggo" />
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: ["image"],
  methods: {
    closeModal() {
      this.$emit("close-modal");
    }
  }
};
</script>

<style scoped>
.modal-backdrop {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-backdrop > img {
  max-height: 70%;
  max-width: 95%;
  align-self: center;
  margin: auto;
  border: 5px solid white;
  /* object-fit: fill; */
}

@media (min-width: 50rem) {
  .modal-backdrop > img {
    max-width: 60%;
  }
}
</style>