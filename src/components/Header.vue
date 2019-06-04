<template>
  <div>
    <div @click="initField(10, 10, 10)" :class="face" class="center"></div>
  </div>
</template>

<script>
import { eventBus } from "../main";

export default {
  data() {
    return {
      face: {
        face_unpressed: true,
        face_lose: false,
        face_win: false,
        size_of_face: true
      }
    };
  },

  methods: {
    initField(rows, cols, mine) {
      eventBus.$emit("initField", { rows, cols, mine });
    }
  }
};
</script>

<style scoped>
.center {
  margin: 0 auto;
}

.face_unpressed {
  background-image: url("../../public/images/face_unpressed.svg");
}

.face_lose {
  background-image: url("../../public/images/face_lose.svg");
}

.face_win {
  background-image: url("../../public/images/face_win.svg");
}

.size_of_face {
  width: 50px;
  height: 50px;
  background-size: 50px 50px;
}
</style>
