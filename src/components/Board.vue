<template>
  <div class="board">
    <div>{{message}}</div>
    <div>turn is: {{turnString}}</div>
    <div v-for="(cellRows, y) in cells" v-bind:key="y">
      <span
        v-for="(cell, x) in cellRows"
        v-bind:class="cellClass(x, y)"
        v-on:click="onClickCell(x, y)"
        v-bind:key="x"
      >{{cell}}</span>
    </div>
  </div>
</template>

<script>
const CELL_TYPE = {
  EMPTY: 0,
  WHITE: 1,
  BLACK: 2
};

export default {
  name: "Board",
  props: {
    size: {
      type: Number,
      required: true,
      default: 8
    }
  },
  data() {
    return {
      message: "",
      turn: CELL_TYPE.BLACK,
      cells: []
    };
  },
  computed: {
    turnString() {
      return this.turn == CELL_TYPE.BLACK ? "black" : "white";
    }
  },
  methods: {
    cellClass(x, y) {
      var cls = "cell.empty";
      switch (this.cells[y][x]) {
        case CELL_TYPE.EMPTY:
          cls = "cell-empty";
          break;
        case CELL_TYPE.WHITE:
          cls = "cell-white";
          break;
        case CELL_TYPE.BLACK:
          cls = "cell-black";
          break;
        default:
          break;
      }
      return cls;
    },
    onClickCell(x, y) {
      switch (this.cells[y][x]) {
        case CELL_TYPE.EMPTY:
          this.cells[y][x] = this.turn;
          if (this.isWinThisTurn(x, y)) {
            this.message = `${this.turnString} is Won!`;
            return;
          }
          this.toggleTurn();
          break;
        default:
          this.message = "can't place here";
          break;
      }
    },
    toggleTurn() {
      this.turn =
        this.turn == CELL_TYPE.BLACK ? CELL_TYPE.WHITE : CELL_TYPE.BLACK;
    },
    isWinThisTurn(x, y) {
      let topLeft2BottomRight = this.scan(x, y, 1, 1);
      let bottomLeft2TopRight = this.scan(x, y, -1, 1);
      let horizontal = this.scan(x, y, 1, 0);
      let vertical = this.scan(x, y, 0, 1);
      let scanList = [
        topLeft2BottomRight,
        bottomLeft2TopRight,
        horizontal,
        vertical
      ];
      for (let i = 0; i < scanList.length; i++) {
        if (scanList[i] === 5) {
          return true;
        }
      }
      return false;
    },
    scan(x, y, moveX, moveY) {
      let n = 1;
      for (let i = 1; i < 5; i++) {
        if (this.cells[y + moveY * i][x + moveX * i] == this.turn) {
          n += 1;
        } else {
          break;
        }
      }
      for (let i = 1; i < 5; i++) {
        if (this.cells[y + -1 * moveY * i][x + -1 * moveX * i] == this.turn) {
          n += 1;
        } else {
          break;
        }
      }
      return n;
    }
  },
  created() {
    var cols = new Array(this.size).fill(CELL_TYPE.EMPTY);
    cols.forEach(
      (_, i) => (cols[i] = new Array(this.size).fill(CELL_TYPE.EMPTY))
    );
    this.cells = cols;
    /*
    this.cells[0][0] = CELL_TYPE.WHITE;
    this.cells[0][1] = CELL_TYPE.BLACK;
    this.cells[1][0] = CELL_TYPE.BLACK;
    this.cells[1][1] = CELL_TYPE.WHITE;
    */
  }
};
</script>

<style scoped lang="scss">
.board {
  .cell-empty {
    background-color: #ccc;
  }
  .cell-white {
    background-color: #fff;
  }
  .cell-black {
    background-color: #000;
  }
}
</style>