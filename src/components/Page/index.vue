<template>
  <div class="page-container">
    <div class="box" :class="{ display: leftHeardRef }" @click="jumpPage(1)">
      <Icon :type="'arrow-double-left'" />
    </div>
    <div class="box" :class="{ display: leftHeardRef }" @click="reduceNum">
      <Icon :type="'arrow-left'" />
    </div>
    <template v-for="i in getPagers" :key="i.value">
      <div
        class="box"
        :class="{ active: i.value === nowNumRef }"
        @click="jumpPage(i.value)"
      >
        {{ i.label }}
      </div>
    </template>

    <div class="box" :class="{ display: rightHeardRef }" @click="addNum">
      <Icon :type="'arrow-right'" />
    </div>
    <div
      class="box"
      :class="{ display: rightHeardRef }"
      @click="jumpPage(maxPage)"
    >
      <Icon :type="'arrow-double-right'" />
    </div>
  </div>
</template>

<script lang="ts">
import {
  ref, defineComponent, computed, watchEffect,
} from 'vue';
import Icon from '../Icon/index.vue';

function page(maxPage: number, pagerCount = 7, nowPage = 1) {
  interface pargersValue {
    label: string;
    value: number;
  }
  const nowNumRef = ref(nowPage); // 当前页码
  const leftHeardRef = computed(() => nowNumRef.value === 1);
  const rightHeardRef = computed(() => nowNumRef.value === maxPage);
  const addNum = () => {
    if (rightHeardRef.value) return;
    nowNumRef.value = nowNumRef.value + 1 <= maxPage ? nowNumRef.value + 1 : maxPage;
  };
  const reduceNum = () => {
    if (leftHeardRef.value) return;
    nowNumRef.value = nowNumRef.value - 1 >= 1 ? nowNumRef.value - 1 : 1;
  };
  const jumpPage = (num: number) => {
    nowNumRef.value = num;
  };
  const getPagers = computed(() => {
    const arr: pargersValue[] = [];
    if (pagerCount) {
      const num = Math.floor(pagerCount / 2);
      let centerNum: number;
      if (nowNumRef.value - num < 0) {
        centerNum = num;
      } else if (nowNumRef.value + num > maxPage) {
        centerNum = maxPage - num;
      } else {
        centerNum = nowNumRef.value;
      }

      if (centerNum > num) {
        arr.push({
          label: '1',
          value: 1,
        });
        arr.push({
          label: '···',
          value: centerNum - 5 >= 1 ? centerNum - 5 : 1,
        });
        for (let i = num - 1; i >= 0; i -= 1) {
          arr.push({
            label: (centerNum - i).toString(),
            value: centerNum - i,
          });
        }
        if (maxPage - centerNum > num) {
          for (let i = 1; i < num; i += 1) {
            arr.push({
              label: (centerNum + i).toString(),
              value: centerNum + i,
            });
          }
          arr.push({
            label: '···',
            value: centerNum + 5 <= maxPage ? centerNum + 5 : maxPage,
          });
          arr.push({
            label: maxPage.toString(),
            value: maxPage,
          });
        } else {
          for (let i = 1; i <= num; i += 1) {
            arr.push({
              label: (centerNum + i).toString(),
              value: centerNum + i,
            });
          }
        }
      } else {
        for (let i = 1; i <= num; i += 1) {
          arr.push({
            label: i.toString(),
            value: i,
          });
        }
        if (maxPage - centerNum > num) {
          for (let i = 1; i < num; i += 1) {
            arr.push({
              label: (centerNum + i).toString(),
              value: centerNum + i,
            });
          }
          arr.push({
            label: '···',
            value: centerNum + 5 <= maxPage ? centerNum + 5 : maxPage,
          });
          arr.push({
            label: maxPage.toString(),
            value: maxPage,
          });
        } else {
          for (let i = 1; i <= num; i += 1) {
            arr.push({
              label: (centerNum + i).toString(),
              value: centerNum + i,
            });
          }
        }
      }
      return arr;
    }
    for (let i = 1; i <= maxPage; i += 1) {
      arr.push({
        label: i.toString(),
        value: i,
      });
    }
    return arr;
  });
  return {
    nowNumRef,
    leftHeardRef,
    rightHeardRef,
    addNum,
    reduceNum,
    jumpPage,
    getPagers,
  };
}

export default defineComponent({
  props: {
    total: {
      type: Number,
      required: true,
    },
    nowPage: {
      type: Number,
      default: 1,
    },
    pageSize: {
      type: Number,
      default: 10,
    },
    pagerCount: {
      type: Number,
      default: 5,
    },
  },
  components: {
    Icon,
  },
  emits: ['change'],
  setup(props, context) {
    const maxPage = Math.ceil(props.total / props.pageSize);
    const pageClass = page(maxPage, props.pagerCount, props.nowPage);
    watchEffect(() => {
      context.emit('change', pageClass.nowNumRef.value);
    });
    return {
      maxPage,
      ...pageClass,
    };
  },
});
</script>

<style lang="scss"  scoped>
.page-container {
  font-size: 16px;
  display: flex;
  align-items: center;
  cursor: pointer;
  color: #409eff;
  .box {
    width: 25px;
    height: 20px;
    margin: 0 2.5px;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: all 0.3s;
    &:hover {
      color: #000;
    }
    &.active {
      color: #000;
      font-weight: bold;
    }
    &.display {
      color: #eee;
    }
  }
}
</style>
