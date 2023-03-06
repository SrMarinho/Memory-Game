<template>
  <div class="container">
      <div class="rows" v-for="row in rows" :key="row.id">
        <div class="columns">
          <div class="containerCard" v-for="column in columns" :key="column.id">
            <div class="card" @click="revelCard(column - 1, row - 1)">
              <div class="backFaceCard">{{ row }}</div>
              <div class="frontFaceCard"></div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rows: 3,
      columns: 4,
      cards_map: []
    }
  },
  mounted() {
    var myCards = document.getElementsByClassName("card")

    var numbers = []

    for (let index = 0; index < (this.rows * this.columns) / 2; index++) {
      numbers.push(index, index)
    }

    numbers = numbers.sort((a, b) => 0.5 - Math.random());

    for (let index = 0; index < myCards.length; index++) {
      myCards[index].lastChild.innerHTML = numbers[index]
    }

    for (let i = 0; i < this.rows; i++) {
      this.cards_map.push([])
      for (let j = 0; j < this.columns; j++) {
        this.cards_map[i].push(j)
      }
    }
    console.log(this.cards_map)
  },
  methods: {
    revelCard: function (x, y) {
      var myCards = document.getElementsByClassName("card")
      myCards[(y * this.columns) + x].toggleAttribute("cardReveled")
    }
  }
}
</script>

<style>
:root {
  --cardAngle: 0deg;
}

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

.rows {
  display: flex;
  flex-direction: column;
}

.columns {
  display: flex;
  flex-direction: row;
}

.containerCard {
  perspective: 400px;
}

.card {
  transform-style: preserve-3d;
  transform-origin: center center;
  margin: 10px;
  transition: 0.3s ease-in-out;
}

.card[cardReveled] {
  transform: rotateY(180deg);
}

.backFaceCard {
  position: absolute;
  width: 120px;
  height: 150px;
  background-image: url("https://img.freepik.com/vetores-gratis/cor-verde-lineart-de-fundo-padrao_487879-550.jpg?w=900&t=st=1677875918~exp=1677876518~hmac=fa0f324cb7c4916ab21dca551183bed81d140a436f0869faf3488d588ae520fb");
  background-size: contain;
  border-radius: 5px;
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.frontFaceCard {
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  width: 119px;
  height: 149px;
  background-color: black;
  border-radius: 5px;
  font-size: 72px;
  transform: rotateY(180deg);
}

</style>