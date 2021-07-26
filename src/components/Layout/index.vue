<template>
  <div
    class="layout-container"
    :style="{
      minWidth: min + 'px',
    }"
  >
    <div class="left">
      <slot name="left"></slot>
    </div>
    <div class="middle">
      <slot name="middle"></slot>
    </div>
    <div class="right">
      <slot name="right"></slot>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    min: {
      //   最小宽度
      type: Number,
      default: 1200,
    },
  },
});
</script>

<style lang="scss" scoped>
.layout-container {
  width: 100%;
  height: 100%;
  display: flex;
  .left,
  .right {
    flex: 0 0 auto;
    overflow: heidden;
  }
  .middle {
    flex: 1 1 auto;
    overflow: heidden;
  }
}
</style>
