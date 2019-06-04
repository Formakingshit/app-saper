<template>
  <table style="width:100%" :class="{block: block}">
    <tr v-for="(rows, rowsIndex) in fields" :key="rowsIndex">
      <th v-for="(cells, cellIndex) in rows" :key="fields[rowsIndex][cellIndex].id">
        <app-cell
          @click.native="sendCoordinat(cells.coordinats)"
          @mousedown.native="mousedown"
          @mouseup.native="mouseup"
          @click.right.native="flag(cells.cell)"
          :cell="fields[rowsIndex][cellIndex].cell"
          @contextmenu.native.prevent
        ></app-cell>
      </th>
    </tr>
  </table>
</template>

<script>
import { eventBus } from "../main";
import Cell from "./Cell.vue";

export default {
  data() {
    return {
      fields: new Array(),
      block: false
    };
  },
  components: {
    appCell: Cell
  },
  methods: {
    mouseup() {
      eventBus.$emit("changeFace", {
        face_active: false,
        face_unpressed: true
      });
    },
    mousedown() {
      eventBus.$emit("changeFace", {
        face_active: true,
        face_unpressed: false
      });
    },
    show(cell) {
      cell.show = true;
    },
    flag(cell) {
      cell.flag = !cell.flag;
    },
    makeMine: function(rows, cols, mine) {
      let mineIter = 0;

      while (mineIter < mine) {
        const x = Math.floor(Math.random() * rows);
        const y = Math.floor(Math.random() * cols);

        if (!this.fields[x][y].cell.mine) {
          this.fields[x][y].cell.mine = true;
          mineIter++;
        }
      }
    },
    initField({ rows, cols, mine }) {
      eventBus.$emit("changeFace", {
        face_unpressed: true,
        face_lose: false,
        face_win: false
      });
      this.block = false;

      this.fields = new Array();
      for (let i = 0; i < rows; i++) {
        this.fields[i] = new Array();
        for (let j = 0; j < cols; j++) {
          this.fields[i].push({
            coordinats: {
              x: i,
              y: j
            },
            cell: {
              flag: false,
              show: false,
              mine: false,
              mine_red: false,
              mine_wrong: false,
              one: false,
              two: false,
              three: false,
              four: false,
              five: false,
              six: false,
              seven: false,
              eighth: false
            },
            id: Math.random()
          });
        }
      }
      this.makeMine(rows, cols, mine);
    },
    sendCoordinat({ x: xCoordinats, y: yCoordinats }) {
      const length = this.fields.length;

      if (this.fields[xCoordinats][yCoordinats].cell.mine) {
        for (let i = 0; i < length; i++) {
          for (let j = 0; j < length; j++) {
            if (this.fields[i][j].cell.mine) {
              this.showCell(i, j);
              if (this.fields[i][j].cell.flag) {
                this.fields[i][j].cell.flag = false;
                this.fields[i][j].cell.mine = false;
                this.fields[i][j].cell.mine_wrong = true;
              }
            }
          }
        }

        eventBus.$emit("changeFace", {
          face_unpressed: false,
          face_lose: true,
          face_win: false
        });

        this.fields[xCoordinats][yCoordinats].cell.mine = false;
        this.fields[xCoordinats][yCoordinats].cell.mine_red = true;
        this.block = true;
      } else {
        this.setNumberNearClick(xCoordinats, yCoordinats);

        const final = length * length;
        let countKnown = 0;

        for (let i = 0; i < length; i++) {
          for (let j = 0; j < length; j++) {
            if (
              this.fields[i][j].cell.show ||
              this.fields[i][j].cell.flag ||
              this.fields[i][j].cell.one ||
              this.fields[i][j].cell.two ||
              this.fields[i][j].cell.three ||
              this.fields[i][j].cell.four ||
              this.fields[i][j].cell.five ||
              this.fields[i][j].cell.six ||
              this.fields[i][j].cell.seven ||
              (this.fields[i][j].cell.eighth && !this.fields[i][j].cell.mine)
            ) {
              countKnown++;
            }
          }
        }

        if (countKnown == final) {
          eventBus.$emit("changeFace", {
            face_unpressed: false,
            face_lose: false,
            face_win: true
          });
        }
      }
    },
    setNumberNearClick(xCoordinats, yCoordinats) {
      const length = this.fields.length;
      let count = 0;

      for (let i = 1; i >= -1; i--) {
        for (let j = 1; j >= -1; j--) {
          if (!(i == 0 && j == 0)) {
            count += this.getNumber(xCoordinats - i, yCoordinats - j);
          }
        }
      }

      if (count) {
        this.setNumber(count, xCoordinats, yCoordinats);
      }
      if (
        !count &&
        !this.fields[xCoordinats][yCoordinats].cell.mine &&
        !this.fields[xCoordinats][yCoordinats].cell.show
      ) {
        this.showCell(xCoordinats, yCoordinats);
        this.fields[xCoordinats][yCoordinats].cell.flag = false;

        if (xCoordinats > 0 && yCoordinats > 0) {
          this.setNumberNearClick(xCoordinats - 1, yCoordinats - 1);
        }

        if (xCoordinats > 0 && yCoordinats < length - 1) {
          this.setNumberNearClick(xCoordinats - 1, yCoordinats + 1);
        }

        if (xCoordinats < length - 1 && yCoordinats < length - 1) {
          this.setNumberNearClick(xCoordinats + 1, yCoordinats + 1);
        }

        if (xCoordinats < length - 1 && yCoordinats > 0) {
          this.setNumberNearClick(xCoordinats + 1, yCoordinats - 1);
        }

        if (xCoordinats > 0) {
          this.setNumberNearClick(xCoordinats - 1, yCoordinats);
        }

        if (xCoordinats < length - 1) {
          this.setNumberNearClick(xCoordinats + 1, yCoordinats);
        }

        if (yCoordinats > 0) {
          this.setNumberNearClick(xCoordinats, yCoordinats - 1);
        }

        if (yCoordinats < length - 1) {
          this.setNumberNearClick(xCoordinats, yCoordinats + 1);
        }
      }
    },
    getNumber(xCoordinats, yCoordinats) {
      const length = this.fields.length;

      if (
        xCoordinats < 0 ||
        yCoordinats < 0 ||
        xCoordinats >= length ||
        yCoordinats >= length
      ) {
        return false;
      }

      return this.fields[xCoordinats][yCoordinats].cell.mine;
    },
    setNumber(count, xCoordinats, yCoordinats) {
      const field = this.fields[xCoordinats][yCoordinats];

      if (count == 1) {
        field.cell.one = true;
      } else if (count == 2) {
        field.cell.two = true;
      } else if (count == 3) {
        field.cell.three = true;
      } else if (count == 4) {
        field.cell.four = true;
      } else if (count == 5) {
        field.cell.five = true;
      } else if (count == 6) {
        field.cell.six = true;
      } else if (count == 7) {
        field.cell.seven = true;
      } else if (count == 8) {
        field.cell.eighth = true;
      }
    },
    showCell(xCoordinats, yCoordinats) {
      this.fields[xCoordinats][yCoordinats].cell.show = true;
    }
  },
  mounted() {
    this.initField({ rows: 10, cols: 10, mine: 10 });
    eventBus.$on("initField", data => {
      this.initField(data);
    });
  }
};
</script>

<style scoped>
.block {
  pointer-events: none;
}
</style>
