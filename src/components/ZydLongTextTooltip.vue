<template>
  <div class="long-text-tooltip-wrap">
    <el-tooltip
      ref="tlp"
      :content="text"
      effect="dark"
      :disabled="!tooltipFlag"
      :placement="placement"
      theme="light"
      v-if="!disabled"
    >
      <p class="over-flow" style="cursor: pointer" @mouseenter="visibilityChange($event)">
        {{ text }}
      </p>
    </el-tooltip>
    <p v-else class="over-flow">{{ text }}</p>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  defineProps({
    /**
     * 显示的内容
     */
    text: {
      type: String,
      default: ''
    },
    /**
     * Tooltip 的出现位置
     * @values top/top-start/top-end/bottom/bottom-start/bottom-end/left/left-start/left-end/right/right-start/right-end
     */
    placement: {
      type: String,
      default: 'top-start'
    },
    /**
     * 是否需要tooltip
     * @deprecated 下个版本不再使用，将使用visible替换
     */
    disabled: {
      default: false,
      type: Boolean,
    },
  });

  const tooltipFlag = ref(false);
  
  const visibilityChange = (event) => {
    const ev = event.target;
    const ev_weight = ev.scrollWidth; // 文本的实际宽度
    const content_weight = ev.parentNode.clientWidth; // 文本容器宽度(父节点)
    if (ev_weight > content_weight) {
    // 文本宽度 > 实际内容宽度  =》内容溢出
      tooltipFlag.value = true;
    } else {
    // 否则为不溢出
      tooltipFlag.value = false;
    }
  };
</script>

<style lang="scss" scoped>
.long-text-tooltip-wrap {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  height: 24px;
}

.over-flow {
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  word-break: break-all;
}

p {
  width: 100%;
  margin: 0;
}
</style>
