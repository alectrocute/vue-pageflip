<template>
  <div ref="htmlElementRef" :class="className" :style="style">
    <slot />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onBeforeUnmount, nextTick } from 'vue';
import { PageFlip } from 'page-flip';
import type { IFlipSetting } from './settings';

export default defineComponent({
  name: 'HTMLFlipBook',
  props: {
    className: {
      type: String,
      required: true,
    },
    style: {
      type: Object as () => Record<string, any>,
      required: true,
    },
    renderOnlyPageLengthChange: {
      type: Boolean,
      default: false,
    },
    // PageFlip settings (expand as needed)
    width: Number,
    height: Number,
    size: String,
    minWidth: Number,
    maxWidth: Number,
    minHeight: Number,
    maxHeight: Number,
    drawShadow: Boolean,
    maxShadowOpacity: Number,
    showCover: Boolean,
    mobileScrollSupport: Boolean,
    useMouseEvents: Boolean,
    swipeDistance: Number,
    clickEventForward: Boolean,
    usePortrait: Boolean,
    startPage: Number,
    autoSize: Boolean,
  },
  emits: ['flip', 'changeOrientation', 'changeState', 'init', 'update'],
  setup(props, { emit, expose }) {
    const htmlElementRef = ref<HTMLElement | null>(null);
    const pageFlip = ref<PageFlip | null>(null);
    const childRefList = ref<HTMLElement[]>([]);

    expose({ pageFlip });

    const removeHandlers = () => {
      pageFlip.value?.off('flip');
      pageFlip.value?.off('changeOrientation');
      pageFlip.value?.off('changeState');
      pageFlip.value?.off('init');
      pageFlip.value?.off('update');
    };

    const setHandlers = () => {
      if (!pageFlip.value) return;

      pageFlip.value.on('flip', (e) => emit('flip', e));
      pageFlip.value.on('changeOrientation', (e) => emit('changeOrientation', e));
      pageFlip.value.on('changeState', (e) => emit('changeState', e));
      pageFlip.value.on('init', (e) => emit('init', e));
      pageFlip.value.on('update', (e) => emit('update', e));
    };

    const refreshOnPageDelete = () => {
      pageFlip.value?.clear();
    };

    onMounted(async () => {
      await nextTick(); // wait for slot content

      if (!htmlElementRef.value) return;

      const children = Array.from(htmlElementRef.value.children) as HTMLElement[];

      if (
        !props.renderOnlyPageLengthChange ||
        children.length !== childRefList.value.length
      ) {
        if (children.length < childRefList.value.length) {
          refreshOnPageDelete();
        }

        childRefList.value = children;
      }

      if (!pageFlip.value) {
        pageFlip.value = new PageFlip(htmlElementRef.value, props as IFlipSetting);
      }

      if (!pageFlip.value.getFlipController()) {
        pageFlip.value.loadFromHTML(childRefList.value);
      } else {
        pageFlip.value.updateFromHtml(childRefList.value);
      }

      setHandlers();
    });

    onBeforeUnmount(() => {
      removeHandlers();
      pageFlip.value?.destroy();
    });

    return {
      htmlElementRef,
      className: props.className,
      style: props.style,
    };
  },
});
</script>
