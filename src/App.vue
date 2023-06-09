<template>
  <div class="game">
    <div class="game-container">
      <h1>GAME XẾP CHỮ</h1>
      <div class="game-container-time">{{ time }}</div>
      <div class="game-container-play">
        <button class="play" @click="onClickPlayGame()">Click to play</button>
      </div>
      <div class="game-container-puzzle">
        <input
          @click="onClickMove($event)"
          value="1"
          class="cell-puzzle row-1 col-1"
          type="button"
          ref="input1"
        />
        <input
          @click="onClickMove($event)"
          value="2"
          class="cell-puzzle row-1 col-2"
          type="button"
          ref="input2"
        />
        <input
          @click="onClickMove($event)"
          value="3"
          class="cell-puzzle row-1 col-3"
          type="button"
          ref="input3"
        />
        <input
          @click="onClickMove($event)"
          value="4"
          class="cell-puzzle row-1 col-4"
          type="button"
          ref="input4"
        />
        <input
          @click="onClickMove($event)"
          value="5"
          class="cell-puzzle row-2 col-1"
          type="button"
          ref="input5"
        />
        <input
          @click="onClickMove($event)"
          value="6"
          class="cell-puzzle row-2 col-2"
          type="button"
          ref="input6"
        />
        <input
          @click="onClickMove($event)"
          value="7"
          class="cell-puzzle row-2 col-3"
          type="button"
          ref="input7"
        />
        <input
          @click="onClickMove($event)"
          value="8"
          class="cell-puzzle row-2 col-4"
          type="button"
          ref="input8"
        />
        <input
          @click="onClickMove($event)"
          value="9"
          class="cell-puzzle row-3 col-1"
          type="button"
          ref="input9"
        />
        <input
          @click="onClickMove($event)"
          value="10"
          class="cell-puzzle row-3 col-2"
          type="button"
          ref="input10"
        />
        <input
          @click="onClickMove($event)"
          value="11"
          class="cell-puzzle row-3 col-3"
          type="button"
          ref="input11"
        />
        <input
          @click="onClickMove($event)"
          value="12"
          class="cell-puzzle row-3 col-4"
          type="button"
          ref="input12"
        />
        <input
          @click="onClickMove($event)"
          value="13"
          class="cell-puzzle row-4 col-1"
          type="button"
          ref="input13"
        />
        <input
          @click="onClickMove($event)"
          value="14"
          class="cell-puzzle row-4 col-2"
          type="button"
          ref="input14"
        />
        <input
          @click="onClickMove($event)"
          value="15"
          class="cell-puzzle row-4 col-3"
          type="button"
          ref="input15"
        />
        <input
          @click="onClickMove($event)"
          value="0"
          class="cell-puzzle row-4 col-4 cell-puzzle-zero"
          type="button"
          ref="input0"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  data() {
    return {
      matrix: [
        [1, 2, 3, 4],
        [5, 6, 7, 8],
        [9, 10, 11, 12],
        [13, 14, 15, 0],
      ],
      secs: 0,
      currentSeconds: 0,
      currentMinutes: 0,
      time: "",
      timer: 0,
    };
  },

  methods: {
    intervalTime() {
      this.currentMinutes = Math.floor(this.secs / 60);
      this.currentSeconds = this.secs % 60;

      if (this.currentMinutes <= 9) {
        this.currentMinutes = "0" + this.currentMinutes;
      }

      if (this.currentSeconds <= 9) {
        this.currentSeconds = "0" + this.currentSeconds;
      }

      this.secs++;
      this.time = this.currentMinutes + ":" + this.currentSeconds;
      this.timer = setTimeout(() => {
        this.intervalTime();
      }, 1000);
    },
    beginTime() {
      this.secs = 0;
      this.currentSeconds = 0;
      this.currentMinutes = 0;
      clearTimeout(this.timer);
      this.intervalTime();
    },
    checkWin() {
      let winString = "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,0";
      let matrixStr = this.matrix.toString();
      if (winString == matrixStr) {
        Swal.fire(
          `${this.time}`,
          "You win ! amazing goodjob em !",
          "success"
        ).then(() => {
          clearTimeout(this.timer);
        });
      }
    },
    //Hàm khi click vào mỗi ô input
    move(obj) {
      let numberCell = obj.target.value;
      console.log(numberCell);
      let win = false;
      let input = `input${numberCell}`;
      for (var i = 0; i < 4; i++) {
        for (var j = 0; j < 4; j++) {
          if (this.matrix[i][j] == numberCell) {
            if (j > 0 && this.matrix[i][j - 1] == 0) {
              this.$refs.input0.style.left = j * 60 + "px";
              this.$refs[input].style.left = (j - 1) * 60 + "px";

              this.matrix[i][j - 1] = numberCell;
              this.matrix[i][j] = 0;
            } else if (j < 3 && this.matrix[i][j + 1] == 0) {
              this.$refs.input0.style.left = j * 60 + "px";
              this.$refs[input].style.left = (j + 1) * 60 + "px";

              this.matrix[i][j + 1] = numberCell;
              this.matrix[i][j] = 0;
            } else if (i > 0 && this.matrix[i - 1][j] == 0) {
              this.$refs.input0.style.top = i * 60 + "px";
              this.$refs[input].style.top = (i - 1) * 60 + "px";

              this.matrix[i - 1][j] = numberCell;
              this.matrix[i][j] = 0;
            } else if (i < 3 && this.matrix[i + 1][j] == 0) {
              this.$refs.input0.style.top = i * 60 + "px";
              this.$refs[input].style.top = (i + 1) * 60 + "px";

              this.matrix[i + 1][j] = numberCell;
              this.matrix[i][j] = 0;
            }

            win = this.checkWin();
            if (win) {
              break;
            }
            return;
          }
        }
      }
    },
    onClickMove(e) {
      if (this.secs >= 0) {
        this.move(e);
      }
    },
    // Hàm tạo ra một ma trận ramdom 15 phần tử phân biệt từ 1 đến 15
    shuffleMatrix() {
      let arr = [];
      while (arr.length < 15) {
        var r = Math.floor(Math.random() * 15) + 1;
        if (arr.indexOf(r) === -1) arr.push(r);
      }
      return arr;
    },
    initialize() {
      let arrRamdomArray = this.shuffleMatrix(); // Tạo một ma trận ramdom
      let count = 0;
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          let number = arrRamdomArray[count];
          this.matrix[i][j] = number;

          if (i == 3 && j == 3) {
            this.matrix[i][j] = 0;
          }
          // console.log(number);
          let input = `input${number}`;
          if (this.$refs[input]) {
            this.$refs[input].style.top = i * 60 + "px";
            this.$refs[input].style.left = j * 60 + "px";
          }
          count++;
        }
      }
    },
    onClickPlayGame() {
      this.initialize(); //Hàm khởi tạo chạy game
      this.beginTime(); // Hàm tính thời gian
    },
  },
};
</script>

<style lang="scss" scoped>
.game {
  width: 100%;
  margin: 0 auto;

  a {
    text-align: center;
  }
  &-container {
    font-size: 16px;
    width: 300px;
    margin: 50px auto 0;
    text-align: center;

    h1 {
      color: #276d8c;
    }

    &-puzzle {
      height: 242px;
      width: 242px;
      position: relative;
      border: 2px solid #757575;
      margin: 0 auto 25px;

      .cell-puzzle {
        width: 58px;
        height: 58px;
        border: 2px solid white;
        position: absolute;
        text-align: center;
        vertical-align: middle;
        line-height: 20px;
        font-size: 36px;
        line-height: 60px;
        background-color: #3b9aff;
        z-index: 100;
        cursor: pointer;
        user-select: none;
      }
      .cell-puzzle-zero {
        background-color: white !important;
        z-index: 50;
        color: #ffffff;
      }
      > .row-1 {
        top: 0;
      }
      > .row-2 {
        top: 60px;
      }
      > .row-3 {
        top: 120px;
      }
      > .row-4 {
        top: 180px;
      }
      > .col-1 {
        left: 0;
      }
      > .col-2 {
        left: 60px;
      }
      > .col-3 {
        left: 120px;
      }
      > .col-4 {
        left: 180px;
      }
    }
    &-play {
      margin: 10px 0;
      text-align: center;
      height: 50px;
      .play {
        width: 150px;
        height: 50px;
        border: 2px solid #3f4f2e;
        text-align: center;
        text-decoration: none;
        font-weight: 600;
        font-size: 20px;
        background-color: #689b37;
        display: inline-block;
        color: white;
        text-align: center;
        line-height: 48px;
        border-radius: 3px;
      }
    }

    &_puzzle {
      font-size: 16px;
      color: coral;
    }
  }
}
</style>
