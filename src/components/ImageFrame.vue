<template>
  <b-card-group deck>
    <b-card class="shadow-sm bg-dark" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0 text-center" footer-class="p-1 pl-2 pr-2">
      <b-card-img :style="imgStyle" :class="imgClass" :src="src" @click="$emit('select', obj)"/>
      <template v-slot:footer>
        <span class="text-light">{{caption}}</span>
        <b-button class="float-right" v-if="destroyButton" variant="danger" size="sm" @click="destroy(obj)">
          Delete
        </b-button>
      </template>
    </b-card>
  </b-card-group>
</template>

<script>
export default {
  name: "ImageFrame",
  props: {
    src: String,
    caption: String,
    obj: String,
    destroyButton: Boolean,
    imgClass: String,
    imgStyle: String,
  },
  methods: {
    destroy(obj) {
      if (window.confirm("Delete?")) {
        this.$emit("destroy", obj);
      }
    },
  },
};
</script>

<style scoped>

</style>