<template>
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
      rows: 0,
      columns: 0,
      cards_map: [],
      cardsReveledMap: [],
      selectedCards: [],
      previewsSelectedCard: []
    }
  },
  mounted() {
    var cards = document.getElementsByClassName("card")
    
    this.rows = 3
    this.columns = 4

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
  methods: {
    revelCard: function (row, column) {
      var count = 0
      this.selectedCards[column][row] = true
      this.selectedCards.forEach(element => {
        element.forEach(item => {
          item == true? count += 1 : ''
          this.previewsSelectedCard = [column, row]
        })
      });
      if (count == 2) {
        setTimeout(() => {
          for (let i = 0; i < this.selectedCards.length; i++) {
            for (let j = 0; j < this.selectedCards[i].length; j++) {
              this.selectedCards[i][j] = false
              if (this.previewsSelectedCard[0] == this.selectedCards[i][j]) {
                
              }
            }
          }
        }, 500);
      }
      // this.cardsReveledMap[column][row] = true
    },
    shuffle: function (matrix) {
      for (let i = 0; i < matrix.length; i++) {
        var j = Math.floor(Math.random() *  i);
        var aux = matrix[i]
        matrix[i] = matrix[j]
        matrix[j] = aux
      }
      return matrix
    }
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
  background-color: #303030;
  display: flex;
  justify-content: center;
  align-items: center;
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
  background-image: linear-gradient(45deg, #00FFFF, #FF00FF);
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