<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

declare global {
  const fs: any
}

const handleDrop = async (e: DragEvent): Promise<void> => {
  e.preventDefault()

  if (e.dataTransfer) {
    for (const f of e.dataTransfer.files) {
      const fd = fs.openSync(f.path, 'r')
      if (fd < 0) {
        throw new Error('open file failed')
      }

      console.log('read ', f.path, ' ok')
      const v = new Uint8Array(4)
      const read_bytes = fs.readSync(fd, v, 0, 4, -1)
      if (read_bytes != 4) {
        throw new Error('read file data failed')
      }

      console.log('read data', v)

    }
  }
}

onMounted(() => {

})

onUnmounted(() => {
})

defineExpose({

})
</script>

<template>
  <div class="player" @drop="handleDrop" @dragenter.prevent @dragover.prevent>
    <video ref="playerRef" class="video-js"></video>
  </div>
</template>

<style>
@import '../assets/css/styles.less';

.player {
  flex: 1;
}
</style>
