<template>
  <el-container class="emqx-container" :class="normal ? 'is-normal' : ''">
    <template v-if="normal">
      <emqx-aside :width="`${navWidth}px`">
        <div v-if="fixedNav" class="nav-container" ref="navContainer" :style="`width: ${navWidth}px`">
          <slot name="nav"></slot>
        </div>
        <slot v-else name="nav"></slot>
      </emqx-aside>
      <emqx-main>
        <slot name="page-content"></slot>
      </emqx-main>
    </template>
    <slot v-else></slot>
  </el-container>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'
import { ElContainer } from 'element-plus'

export default defineComponent({
  name: 'EmqxContainer',
  components: {
    ElContainer,
  },
  props: {
    /**
     * is it normal layout that left is nav and right is page content
     * and then the width will change by the window's width
     */
    normal: {
      type: Boolean,
      default: false,
    },
    fixedNav: {
      type: Boolean,
      default: false,
    },
    navWidth: {
      type: Number,
      default: 220,
    },
  },
  setup(props) {
    const navContainer = ref<HTMLElement>()

    onMounted(() => {
      if (!props.normal || !props.fixedNav) {
        return
      }
      const ele: HTMLElement | undefined = navContainer?.value
      const posY = ele?.offsetTop
      ele?.style?.setProperty('position', 'fixed')
      ele?.style?.setProperty('top', posY ? `${posY}px` : '0px')
    })
    return {
      navContainer,
    }
  },
})
</script>
