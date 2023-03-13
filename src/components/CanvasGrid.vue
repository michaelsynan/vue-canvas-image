<template>
  <div>
    <h1 class="app-title">Canvas Example</h1>
    <div class="columns-container">
      <div v-for="(row, rowIndex) in rows" :key="rowIndex" class="row">
        <div v-for="(column, columnIndex) in columns" :key="columnIndex" :id="'column-' + columnIndex + '-row-' + rowIndex" class="column">
          <canvas :ref="'canvas-' + columnIndex + '-row-' + rowIndex" :width="columnWidth" :height="rowHeight" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  props: {
    columns: {
      type: Number,
      default: 5,
    },
    rows: {
      type: Number,
      default: 3,
    },
    image: {
      type: String,
      default: '/hudl.png',
    },
  },
  data() {
    return {
      canvasWidth: 0,
      canvasHeight: 0,
      columnWidth: 0,
      rowHeight: 0,
    };
  },
  mounted() {
  const image = new Image();
  image.src = this.image;
  image.addEventListener('load', () => {
    this.canvasWidth = image.width;
    this.canvasHeight = image.height;

    // Ensure columnWidth is divisible by columns
    let columnWidth = Math.ceil(this.canvasWidth / this.columns);
    columnWidth += (this.columns - (columnWidth % this.columns)) % this.columns;
    this.columnWidth = columnWidth;

    this.rowHeight = Math.ceil(this.canvasHeight / this.rows);

    const columnImages = [];
    for (let i = 0; i < this.columns; i++) {
      columnImages[i] = [];
      for (let j = 0; j < this.rows; j++) {
        columnImages[i][j] = new Image();
        columnImages[i][j].src = this.image;
        columnImages[i][j].addEventListener('load', () => {
          const canvas = this.$refs['canvas-' + i + '-row-' + j][0];
          const context = canvas.getContext('2d');
          context.drawImage(columnImages[i][j], i * this.columnWidth, j * this.rowHeight, this.columnWidth, this.rowHeight, 0, 0, this.columnWidth, this.rowHeight);
        });
      }
    }
  });
},

};
</script>

<style scoped>
.app-title {
  font-size: 2rem;
  text-align: center;
  margin-top: 2rem;
}

.columns-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.row {
  display: flex;
  margin-bottom: 0px;
}

.column {
  display: flex;
  justify-content: center;
  align-items: center;
  width: calc((100% / {{columns}}) - {{1/columns}}px);
  max-width: calc((100% / {{columns}}) - {{1/columns}}px);
}

.column:nth-child(even) {
  margin-top: 0px;
}
</style>
