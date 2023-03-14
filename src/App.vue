<template>
  <div :class="running == true? 'menu running' : 'menu'">
    <h1 ref="game_title">Memory Game</h1>
    <div class="start" @click="running = true; init_game()">Start</div>
    <div class="option">
      <div class="changeDifficulty" @click="difficulty > 0? difficulty-- : ''">-</div>
      <div class="difficulty">{{ difficulty_names[difficulty] }}</div>
      <div class="changeDifficulty" @click="difficulty < difficulty_names.length - 1? difficulty++ : ''">+</div>
    </div>
  </div>
  <div class="container">
    <div class="cardBox" v-for="row in rows" :key="row.id">
      <div class="card" v-for="column in columns" :key="column.id" @click="revelCard(column - 1, row - 1)" :cardReveled="cardsReveledMap[row - 1][column - 1] || selectedCards[row - 1][column - 1]">
        <div class="frontCard">{{ cards_map[((row - 1) * columns) + (column - 1)] }}</div>
        <div class="backCard"></div>
      </div>
    </div>
  </div>  
</template>

<script>
export default {
  data() {
    return {
      running: false,
      rows: 0,
      columns: 0,
      cards_map: [],
      cardsReveledMap: [],
      selectedCards: [],
      previewsSelectedCard: null,
      difficulty: 1,
      difficulty_names: ["Easy", "Medium", "Hard"]
    }
  },
  mounted() {
    
  },
  methods: {
    init_game: function() {
      if (this.difficulty == 0) {
        this.rows = 3
        this.columns = 4
      } else if (this.difficulty == 1) {
        this.rows = 4
        this.columns = 5
      } else if (this.difficulty == 2) {
        this.rows = 4
        this.columns = 8
      }

      this.cards_map = []
      this.cardsReveledMap = []
      this.selectedCards = []
      this.previewsSelectedCard = null

      for (let i = 0; i < (this.rows * this.columns) / 2; i++) {
        this.cards_map.push(i, i)
      }

      for (let i = 0; i < this.rows; i++) {
        this.cardsReveledMap.push([])
        this.selectedCards.push([])
        for (let j = 0; j < this.columns; j++) {
          this.cardsReveledMap[i].push(false)
          this.selectedCards[i].push(false)
        }
      }

      this.cards_map = this.shuffle(this.cards_map)   
    },
    revelCard: function (row, column) {
      var count = 0
      this.selectedCards[column][row] = true
      this.selectedCards.forEach(element => {
        element.forEach(item => {
          item == true? count += 1 : ''
          if (this.previewsSelectedCard == null) {
            this.previewsSelectedCard = [column, row]
          }
        })
      });

      if (count == 2) {
        setTimeout(() => {
          for (let i = 0; i < this.selectedCards.length; i++) {
            for (let j = 0; j < this.selectedCards[i].length; j++) {
              this.selectedCards[i][j] = false
            }
          }
        }, 500);

        if (this.cards_map[(column * this.columns) + row] == this.cards_map[(this.previewsSelectedCard[0] * this.columns) + this.previewsSelectedCard[1]]) {
          this.cardsReveledMap[this.previewsSelectedCard[0]][this.previewsSelectedCard[1]] = true
          this.cardsReveledMap[column][row] = true
        }
        
        this.previewsSelectedCard = null

        var game_check = false

        for (let i = 0; i < this.cardsReveledMap.length; i++) {
          for (let j = 0; j < this.cardsReveledMap[i].length; j++) {
            if (this.cardsReveledMap[i][j] == false) {
              game_check = false
              break
            } else {
              game_check = true
            }
          }
        }

        if (game_check) {
          this.running = false
          this.$refs.game_title.innerHTML = "Você ganhou"
        }
      }

    },
    shuffle: function (matrix) {
      for (let i = 0; i < matrix.length; i++) {
        var j = Math.floor(Math.random() *  i);
        var aux = matrix[i]
        matrix[i] = matrix[j]
        matrix[j] = aux
      }
      return matrix
    },
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

body {
  height: 100vh;
  background-color: #202020;
  display: flex;
  justify-content: center;
  align-items: center;
  -webkit-user-select: none; /* Safari */
  -ms-user-select: none; /* IE 10 and IE 11 */
  user-select: none; /* Standard syntax */
}

.menu {
  position: absolute;
  background-image: linear-gradient(45deg, #221940, #5a1b35);
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.running {
  transition: 0.5s;
  opacity: 0;
  visibility: hidden;
}

.menu h1 {
  color: #f94852;
  font-size: 100px;
  margin-bottom: 100px;
}

.menu .start {
  width: 300px;
  height: 100px;
  background-color: #f94852;
  margin: 10px;
  border-radius: 50px;
  font-size: 48px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.menu .start:hover {
  background-color: transparent;
  outline: 5px #f94852 solid;
  color: #f94852;
}

.menu .option {
  display: flex;
}

.menu .option .difficulty {
  width: 200px;
  height: 75px;
  background-color: #f94852;
  margin: 10px;
  border-radius: 50px;
  font-size: 48px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.menu .option .changeDifficulty {
  width: 75px;
  height: 75px;
  background-color: #f94852;
  margin: 10px;
  border-radius: 50px;
  font-size: 48px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.menu .option .changeDifficulty:hover {
  background-color: transparent;
  outline: 5px #f94852 solid;
  color: #f94852;
}

.cardBox {
  perspective: 400px;
  display: flex;
  flex-direction: row;
}

.cardBox .card {
  transform-style: preserve-3d;
  margin: 5px;
  transition: 0.3s ease-in-out;
}

.cardBox .card[cardReveled = true] {
  transform: rotateY(180deg);
}

.cardBox .card .backCard {
  width: 119px;
  height: 149px;
  background-image: linear-gradient(45deg, #381f8d, #df0861);
  border-radius: 10px;
}

.cardBox .card .frontCard {
  position: absolute;
  width: 120px;
  height: 150px;
  background-color: black;
  color: white;
  backface-visibility: hidden;
  transform: rotateY(180deg);
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 72px;
}
</style>