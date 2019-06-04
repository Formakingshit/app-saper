<template>
  <div @click="initField(10, 10, 10)" :class="face" class="center"></div>
</template>

<script>
import { eventBus } from "../main";

export default {
  data() {
    return {
      face: {
        face_unpressed: false,
        face_lose: false,
        face_win: false,
        face_active: false,
        size_of_face: true
      }
    };
  },
  methods: {
    initField(rows, cols, mine) {
      eventBus.$emit("initField", { rows, cols, mine });
    },
    changeFace({
      face_unpressed,
      face_lose,
      face_win ,
      face_active 
    }) {
      this.face.face_unpressed = face_unpressed;
      this.face.face_lose = face_lose;
      this.face.face_win = face_win;
      this.face.face_active = face_active;
    }
  },
  mounted() {
    eventBus.$on("changeFace", data => {
      this.changeFace(data);
    });
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

.face_active {
  background-image: url("../../public/images/face_active.svg");
}

.size_of_face {
  width: 50px;
  height: 50px;
  background-size: 50px 50px;
}
</style>
