<template>
  <div class="flex justify-around items-center flex-col lg:flex-row">
    <div class="flex flex-col items-center bg-white shadow-lg p-6 rounded-lg min-w-[18vw]">
      <h1 class="text-5xl font-bold text-red-400 text-center">X</h1>
      <h1 class="text-xl text-black mt-2">{{ xCount }} Wins</h1>
      <div v-if="currentPlayer === 'X'" class="text-blue-700 mt-2">
        <h1 class="text-lg font-semibold">Your Turn</h1>
      </div>
    </div>

    <table class="bg-gray-400 rounded-lg border-separate border-spacing-4 mt-6">
      <tr v-for="r in 3" :key="r">
        <td
          v-for="c in 3"
          :key="c"
          v-text="board[r - 1][c - 1]"
          @click="turn(r - 1, c - 1)"
          :class="[
            'border-2 border-white w-16 h-16 md:w-20 md:h-20 lg:w-24 lg:h-24 text-2xl md:text-4xl lg:text-6xl cursor-pointer text-center',
            disable || board[r - 1][c - 1] !== '' ? 'pointer-events-none bg-gray-300' : 'bg-white hover:bg-blue-200'
          ]"
        ></td>
      </tr>
    </table>

    <div class="flex flex-col items-center bg-white shadow-lg p-6 rounded-lg min-w-[18vw]">
      <h1 class="text-5xl font-bold text-green-500 text-center">O</h1>
      <h1 class="text-xl text-black mt-2">{{ yCount }} Wins</h1>
      <div v-if="currentPlayer === 'O'" class="text-blue-700 mt-2">
        <h1 class="text-lg font-semibold">Your Turn</h1>
      </div>
    </div>
  </div>

  <div v-if="winner" class="bg-gray-800 text-white p-6 rounded-lg shadow-lg mt-8 text-center max-w-2xl mx-auto">
    <h1 v-if="winner !== 'Draw'" class="text-2xl font-bold">
      Player: {{ winner }} won the match
    </h1>
    <h1 v-else class="text-2xl font-bold">Match Ended in a Draw</h1>
    <div class="flex justify-center gap-6 mt-4">
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="nextGame">
        Next Game
      </button>
      <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded" @click="completeReset">
        Reset Entire Game
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TicTacToe",
  data() {
    return {
      startPlayer: "X",
      currentPlayer: "X",
      xCount: 0,
      yCount: 0,
      board: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],
      winner: "",
      disable: false,
    };
  },

  watch: {
    board: {
      handler: function () {
        const check = (x1, x2, y1, y2, z1, z2) => {
          return (
            this.board[x1][x2] !== "" &&
            this.board[y1][y2] !== "" &&
            this.board[z1][z2] !== "" &&
            this.board[x1][x2] === this.board[y1][y2] &&
            this.board[y1][y2] === this.board[z1][z2]
          );
        };
        const exists = (arr) => {
          return arr.some((row) => row.includes(""));
        };

        const conditionArray = [
          check(0, 0, 0, 1, 0, 2),
          check(1, 0, 1, 1, 1, 2),
          check(2, 0, 2, 1, 2, 2),
          check(0, 0, 1, 0, 2, 0),
          check(0, 1, 1, 1, 2, 1),
          check(0, 2, 1, 2, 2, 2),
          check(0, 0, 1, 1, 2, 2),
          check(0, 2, 1, 1, 2, 0),
        ];

        if (conditionArray.includes(true)) {
          this.winner = this.currentPlayer === "X" ? "O" : "X";
          this.disable = true;
          this.currentPlayer = "Z";
          if (this.winner === "X") this.xCount++;
          else if (this.winner === "O") this.yCount++;
        } else if (!exists(this.board)) {
          this.currentPlayer = "Z";
          this.winner = "Draw";
        }
      },
      deep: true,
    },
  },
  methods: {
    turn(x, y) {
      if (this.currentPlayer === "X") {
        this.board[x][y] = this.currentPlayer;
        this.currentPlayer = "O";
      } else {
        this.board[x][y] = this.currentPlayer;
        this.currentPlayer = "X";
      }
    },
    nextGame() {
      this.disable = false;
      this.board = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.winner = "";
      if (this.startPlayer === "X") {
        this.currentPlayer = "O";
        this.startPlayer = "O";
      } else {
        this.currentPlayer = "X";
        this.startPlayer = "X";
      }
    },
    completeReset() {
      this.disable = false;
      this.board = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.winner = "";
      this.xCount = 0;
      this.yCount = 0;
      this.currentPlayer = "X";
      this.startPlayer = "X";
    },
  },
};
</script>

<style scoped>
</style>
