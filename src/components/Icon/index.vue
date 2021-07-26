<template>
  <i class="iconfont" :class="fontClass"></i>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';

const classMap: { [key: string]: string } = {
  home: 'icon-xianxingzhuye',
  'arrow-down': 'icon-Arrow-down',
  'arrow-right': 'icon-Arrow-right',
  'arrow-double-right': 'icon-Arrow-double-right',
  'arrow-up': 'icon-Arrow-up',
  'arrow-left': 'icon-Arrow-left',
  'arrow-double-left': 'icon-Arrow-double-left',
};

export default defineComponent({
  props: {
    type: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const fontClass = computed(() => classMap[props.type]);
    return {
      fontClass,
    };
  },
});
</script>

<style lang="scss" scoped>
@import url('//at.alicdn.com/t/font_2696265_qcx7950yvii.css');
</style>
