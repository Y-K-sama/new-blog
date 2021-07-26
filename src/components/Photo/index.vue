<template>
  <img
    class="photo-ing"
    :src="url"
    :style="{
      width: width + 'px',
      height: height ? height + 'px' : width + 'px',
      borderRadius: radius ? '50%' : 0,
    }"
    alt=""
  />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    url: {
      type: String,
      required: true,
    },
    width: {
      type: Number,
      required: true,
    },
    height: {
      type: Number,
      default: undefined,
    },
    radius: {
      type: Boolean,
      default: true,
    },
  },
});
</script>

<style lang="scss" scoped>
.photo-img{
  object-fit: cover;
  display: block;
}
</style>
