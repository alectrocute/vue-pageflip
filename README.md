[![GitHub license](https://img.shields.io/github/license/alectrocute/vue-pageflip)](https://github.com/alectrocute/vue-pageflip/blob/master/LICENSE) [![npm](https://img.shields.io/npm/v/vue-pageflip)](https://www.npmjs.com/package/vue-pageflip) [![npm](https://img.shields.io/npm/dm/vue-pageflip)](https://npmcharts.com/compare/vue-pageflip?minimal=true)

# Vue 3 wrapper for StPageFlip

Simple Vue 3 wrapper for StPageFlip library, for creating realistic and beautiful page turning effects.

### Features of StPageFlip

-   Works with simple images on canvas and complex HTML blocks
-   Has simple API and flexible configuration
-   Compatible with mobile devices
-   Supports landscape and portrait screen mode
-   Supports soft and hard page types (only in HTML mode)
-   No dependencies

Live Demo with source code: https://alectrocute.github.io/vue-pageflip/

Docs and examples to StPageFlip: https://alectrocute.github.io/vue-pageflip/

### Installation

You can install the latest version using npm:

`npm install vue-pageflip`

### Basic Usage

```vue
<template>
  <HTMLFlipBook
    className="book"
    :style="{ width: '100%', height: '500px' }"
    :width="600"
    :height="400"
    @flip="onFlip"
    @update="onUpdate"
  >
    <div class="page">Page 1</div>
    <div class="page">Page 2</div>
    <div class="page">Page 3</div>
  </HTMLFlipBook>
</template>

<script setup lang="ts">
import HTMLFlipBook from './HTMLFlipBook.vue';

function onFlip(e: unknown) {
  console.log('Flipped:', e);
}

function onUpdate(e: unknown) {
  console.log('Updated:', e);
}
</script>
```
