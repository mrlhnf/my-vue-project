<template>
  <div class="m-4">
    <el-select
      v-model="value"
      value-key="id"
      placeholder="Select"
      style="width: 240px"
      popper-append-to-body
      @visible-change="handleVisibleChange"
    >
      <el-option
        v-for="item in options.slice(0, optionslimit)"
        :key="item.id"
        :label="item.label"
        :value="item"
      />
    </el-select>
    <p>
      Selected option's description:
      {{ value ? value.desc : 'No selection' }}
    </p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, nextTick } from 'vue';

export default defineComponent({
  data() {
    return {
      value: null as Option | null,
      options: [
        { id: 1, label: 'Option A', desc: 'Option A - 230506' },
        { id: 2, label: 'Option B', desc: 'Option B - 230506' },
        { id: 3, label: 'Option C', desc: 'Option C - 230506' },
        { id: 4, label: 'Option D', desc: 'Option A - 230507' },
        { id: 5, label: 'Option E', desc: 'Option A - 230507' },
        { id: 6, label: 'Option F', desc: 'Option A - 230507' },
        { id: 7, label: 'Option G', desc: 'Option A - 230507' },
        { id: 8, label: 'Option H', desc: 'Option A - 230507' },
        { id: 9, label: 'Option I', desc: 'Option A - 230507' },
        { id: 10, label: 'Option J', desc: 'Option A - 230507' },
      ] as Option[],
      optionslimit: 8,
    };
  },
  methods: {
    handleVisibleChange(visible: boolean) {
      if (visible) {
        nextTick(() => {
          const dropdown = document.querySelector('.el-select-dropdown__wrap');
          if (dropdown) {
            dropdown.addEventListener('scroll', this.handleScroll);
          }
        });
      } else {
        const dropdown = document.querySelector('.el-select-dropdown__wrap');
        if (dropdown) {
          dropdown.removeEventListener('scroll', this.handleScroll);
        }
      }
    },
    handleScroll(event: Event) {
      const target = event.target as HTMLElement;
      if (target.scrollHeight + target.scrollTop >= target.scrollHeight - 10) {
        if (this.optionslimit < this.options.length) {
          this.optionslimit += 2;
        }
      }
    },
  },
});

type Option = {
  id: number;
  label: string;
  desc: string;
};
</script>

<style lang="scss" scoped> // When using scoped, use :deep() to change element plus component style and override global style
:deep(.el-select__wrapper) {
  background-color: transparent;
}
</style>
