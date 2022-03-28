<template>
  <img :src="doggo" alt="Randomly generated doggo" />
</template>

<script>
export default {
  name: "Image",
  props: ["doggo"]
};
</script>

<style>
img {
  width: 100%;
  display: block;
  object-fit: cover;
  aspect-ratio: 1 / 1;
  box-shadow: 0 0.25rem 1rem rgba(0, 0, 0, 0.2);
}
</style>