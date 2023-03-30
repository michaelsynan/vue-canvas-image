<template>
  <div>
    <div class="columns-container">
      <div v-for="(row, rowIndex) in rows" :key="rowIndex" class="row">
        <div v-for="(column, columnIndex) in columns" :key="columnIndex" :id="'column-' + columnIndex + '-row-' + rowIndex" class="column"  :class="{'fade-in': fadeIn}">
          <canvas :ref="'canvas-' + columnIndex + '-row-' + rowIndex" :width="columnWidth" :height="rowHeight" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'canvasImage',
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
    fadeIn: {
      type: Boolean,
      default: true,
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
    image.onload = () => {
      let width = image.width;
      if (width % this.columns !== 0) {
        width += this.columns - (width % this.columns);
      }
      let height = image.height;
      if (height % this.rows !== 0) {
        height += this.rows - (height % this.rows);
      }
      this.canvasWidth = width;
      this.canvasHeight = height;
      this.columnWidth = this.canvasWidth / this.columns;
      this.rowHeight = this.canvasHeight / this.rows;

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
    };
    if (this.fadeIn) {
      const onFadeInEnd = (event) => {
        event.target.removeEventListener('animationend', onFadeInEnd);
        event.target.style.opacity = '1';
      };

      for (let i = 0; i < this.columns; i++) {
        for (let j = 0; j < this.rows; j++) {
          const canvas = this.$refs['canvas-' + i + '-row-' + j][0];
          canvas.addEventListener('animationend', onFadeInEnd);
        }
      }
    }
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
  margin-top: 2px;
}

/* fade in animation */

@keyframes fadeIn {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }


  .fade-in {
    animation: fadeIn 1s ease-out;
  }
</style>
