<template>
  <button class="size_cell" :class="getClass()"></button>
</template>

<script>
export default {
  data() {
    return {
      dataCell: this.cell
    };
  },
  props: ["cell"],
  methods: {
    getClass() {
      if (this.dataCell.mine && this.dataCell.show) {
        return "mine background_size_cell";
      } else if (this.dataCell.mine_red) {
        return "mine_red background_size_cell";
      } else if (this.dataCell.mine_wrong) {
        return "mine_wrong background_size_cell";
      } else if (this.dataCell.one && !this.dataCell.mine) {
        return "one background_size_cell";
      } else if (this.dataCell.two && !this.dataCell.mine) {
        return "two background_size_cell";
      } else if (this.dataCell.three && !this.dataCell.mine) {
        return "three background_size_cell";
      } else if (this.dataCell.four && !this.dataCell.mine) {
        return "four background_size_cell";
      } else if (this.dataCell.five && !this.dataCell.mine) {
        return "five background_size_cell";
      } else if (this.dataCell.six && !this.dataCell.mine) {
        return "six background_size_cell";
      } else if (this.dataCell.seven && !this.dataCell.mine) {
        return "seven background_size_cell";
      } else if (this.dataCell.eighth && !this.dataCell.mine) {
        return "eighth background_size_cell";
      } else if (this.dataCell.show) {
        return "show";
      } else if (this.dataCell.flag) {
        return "flag background_size_cell";
      }
    }
  }
};
</script>

<style>
.size_cell {
  width: 50px;
  height: 50px;
}
.background_size_cell {
  background-size: 50px 50px;
}
.mine {
  background-image: url("../../public/images/mine.svg");
}
.flag {
  background-image: url("../../public/images/flag.svg");
}
.mine_red {
  background-image: url("../../public/images/mine_red.svg");
}

.mine_wrong {
  background-image: url("../../public/images/mine_wrong.svg");
}

.one {
  background-image: url("../../public/images/type1.svg");
}

.two {
  background-image: url("../../public/images/type2.svg");
}

.three {
  background-image: url("../../public/images/type3.svg");
}

.four {
  background-image: url("../../public/images/type4.svg");
}

.five {
  background-image: url("../../public/images/type5.svg");
}

.six {
  background-image: url("../../public/images/type6.svg");
}

.seven {
  background-image: url("../../public/images/type7.svg");
}

.eighth {
  background-image: url("../../public/images/type8.svg");
}

.show {
  background-color: grey;
  pointer-events: none;
}
</style>
