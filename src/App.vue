<template>
  <div id="app">
    <canvas
      ref="can"
      width="800"
      height="600"
      style="border: solid 1px black"
    ></canvas>
    <template v-if="canvas">
      <button
        @click="
          drawingType = null
          canvas.isDrawingMode = false
        "
      >
        Sélection
      </button>
      <button @click="drawingType = 'rectangle'">Rectangle</button>
      <button @click="canvas.isDrawingMode = !canvas.isDrawingMode">
        Dessin libre
      </button>
      <button @click="canvas.remove(canvas.getActiveObject())">
        Supprimer
      </button>
      <button @click="download">Télécharger</button>
      <a ref="download" style="display: none" download="image.jpeg"></a>
      <pre>{{ canvas }}</pre>
    </template>
  </div>
</template>

<script>
import { fabric } from 'fabric'

export default {
  data() {
    return {
      canvas: null,
      drawingType: null,
    }
  },
  mounted() {
    const ref = this.$refs.can
    this.canvas = new fabric.Canvas(ref, {
      isDrawingMode: false,
      backgroundColor: '#fff',
    })
    this.canvas.on('mouse:down', (event) => {
      if (this.drawingType === 'rectangle') {
        const pointer = event.pointer
        const rect = new fabric.Rect({
          left: pointer.x,
          top: pointer.y,
          originX: 'left',
          originY: 'top',
          width: 20,
          height: 20,
          angle: 0,
          fill: 'rgba(255,0,0,0.5)',
          transparentCorners: false,
        })
        this.canvas.add(rect)
      }
    })
  },
  methods: {
    download() {
      const link = this.canvas.toDataURL({ format: 'jpeg' })
      this.$refs.download.setAttribute('href', link)
      this.$refs.download.click()
    },
  },
}
</script>
