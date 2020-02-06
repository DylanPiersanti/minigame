<template>
  <div id="app">
    <div class="content" v-if="win()" >
      <div class="grille">
        <h1>GAGNE !</h1>
      </div>
    </div>
    <div class="content2" v-else>
      <div class="grille">
        <div class="row" v-for="(row, ri) in tab" :key="ri">
          <div class="col" v-for="(col, ci) in row" :key="ci">{{ col }}</div>
        </div>
      </div>
    </div>

    <div class="buttons">
      <button v-if="started" @click="nbCase = 4, greet(), start=true">4x4</button>
      <button v-if="started" @click="nbCase = 6, greet(), start=true">6x6</button>
      <button v-if="started" @click="nbCase = 8, greet(), start=true">8x8</button>

      <button
        v-if="reset"
        @click="nbCase = 0, greet(), reset=false, start=false, started=true"
      >reset</button>
      <button
        v-if="start"
        @click="pion(), advers(), reset=true, start=false, started=false, arrows = true"
      >Lancer le jeu</button>
      <button v-if="arrows" @click="moveUp()">
        <i class="arrow up"></i>
      </button>
      <button v-if="arrows" @click="moveDown()">
        <i class="arrow down"></i>
      </button>
      <button v-if="arrows" @click="moveLeft()">
        <i class="arrow left"></i>
      </button>
      <button v-if="arrows" @click="moveRight()">
        <i class="arrow right"></i>
      </button>
    </div>
  </div>
</template>

<script>
import Vue from "vue";

export default {
  name: "app",
  data: function() {
    return {
      reset: false,
      arrows: false,
      start: false,
      started: true,
      tab: null,
      nbCase: 0,
    };
  },
  methods: {
    greet() {
      this.tab = [];
      for (var i = 0; i < this.nbCase; i++) {
        const row = [];
        this.tab.push(row);
        for (var j = 0; j < this.nbCase; j++) {
          row.push(null);
        }
      }
    },
    pion() {
      const row = Math.floor(Math.random() * this.nbCase);
      const col = Math.floor(Math.random() * this.nbCase);
      Vue.set(this.tab[row], col, "O");
    },
    advers() {
      const row = Math.floor(Math.random() * this.nbCase);
      const col = Math.floor(Math.random() * this.nbCase);
      Vue.set(this.tab[row], col, "X");
    },
    findPion() {
      var tab = this.tab;
      for (var i = 0; i < this.nbCase; i++) {
        var index = tab[i].indexOf("O");
        if (index >= 0) {
          var indexCol = index;
          var indexRow = i;
        }
        console.log(indexCol, indexRow);
      }
      var position = [indexRow, indexCol];
      return position;
    },
    findAdvers() {
      var tab = this.tab;
      for (var i = 0; i < this.nbCase; i++) {
        var index = tab[i].indexOf("X");
        if (index >= 0) {
          var indexCol = index;
          var indexRow = i;
        }
        console.log(indexCol, indexRow);
      }
      var position = [indexRow, indexCol];
      return position;
    },
    moveUp() {
      var position = this.findPion();
      var row = position[0];
      var col = position[1];
      Vue.set(this.tab[row], col, "");
      var newRow = position[0];
      if (row == 0) {
        newRow = position[0];
        newRow = this.nbCase - 1;
      } else {
        newRow = position[0];
        newRow = newRow - 1;
      }

      Vue.set(this.tab[newRow], col, "O");
    },

    moveDown() {
      var position = this.findPion();
      var row = position[0];
      var col = position[1];
      Vue.set(this.tab[row], col, "");
      var newRow = position[0];
      if (row == this.nbCase - 1) {
        newRow = position[0];
        newRow = 0;
      } else {
        newRow = position[0];
        newRow = newRow + 1;
      }

      Vue.set(this.tab[newRow], col, "O");
    },

    moveLeft() {
      var position = this.findPion();
      var row = position[0];
      var col = position[1];
      Vue.set(this.tab[row], col, "");
      var newRow = position[0];
      if (col == 0) {
        var newCol = position[1];
        newCol = this.nbCase - 1;
      } else {
        newCol = position[1];
        newCol = newCol - 1;
      }

      Vue.set(this.tab[newRow], newCol, "O");
    },

    moveRight() {
      var position = this.findPion();
      var row = position[0];
      var col = position[1];
      Vue.set(this.tab[row], col, "");
      var newRow = position[0];
      if (col == this.nbCase - 1) {
        var newCol = position[1];
        newCol = 0;
      } else {
        newCol = position[1];
        newCol = newCol + 1;
      }

      Vue.set(this.tab[newRow], newCol, "O");
    },

    win() {
      var positionPion = this.findPion();
      var positionAdvers = this.findAdvers();
      if (positionAdvers[0] === undefined && positionPion[0]) {
        this.arrows = false;
        return true;
      }
    }
  }
};
</script>

<style>
body {
  background-color: rgb(58, 58, 58);
  color: #fff;
}
#app {
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.buttons {
  position: absolute;
  bottom: 20%;
}
button {
  color: #fff;
  font-size: 25px;
  height: 50px;
  border: none;
  background-color: rgb(16, 147, 253);
  margin: 5px;
  padding: 10px;
  border-radius: 5px;
}
button:hover {
  background-color: orange;
  cursor: pointer;
}
.grille {
  width: 500px;
  height: 500px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 25%;
  border: 1px solid #fff;
  padding: 10px;
  text-align: center;
}
.row {
  flex: 1;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 7px;
}
.col {
  flex: 1;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #eee;
  margin: 5px;
  font-family: sans-serif;
  font-size: 20px;
  font-weight: bold;
}

i {
  border: solid #fff;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}

.right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}

.left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
}

.up {
  transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
}

.down {
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}
</style>
