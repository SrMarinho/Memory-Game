<template>
  <div class="container">
    <div v-for="column in columns" :key="column.id">
      <div class="card" v-for="row in rows" :key="row.id">
        <div class="backFace">{{ cardsNumbersMap[0] != null? cardsNumbersMap[0][0] : '' }}</div>
        <div class="frontFace"></div>
      </div>
    </div>
  </div>
  {{ cardsNumbersMap }}
</template>
<script>
export default {
  data() {
    return {
      rows: 3,
      columns: 4,
      cardsNumbersMap: null
    }
  },
  mounted() {
    this.cardsNumbersMap = []
    for (let column = 0; column < this.columns; column++) {
      this.cardsNumbersMap.push([])
      for (let row = 0; row < this.rows; row++) {
          this.cardsNumbersMap[column].push((Math.random() * 10).toFixed(0));
      }
    }
    console.log(this.cardsNumbersMap[0][0]);
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

body {
  width: 100%;
  height: 100vh;
  background-color: #303030;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  display: flex;
  flex-direction: row;
}

.card {
  position: relative;
  width: 80px;
  height: 100px;
  margin: 5px;
  transform-style: preserve-3d;
  backface-visibility: hidden;
}

.frontFace, .backFace {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80px;
  height: 100px;
  background-color: white;
  border-radius: 5px;
  border: white 1px solid;
}

.frontFace {
  background-image: url("https://img.freepik.com/vetores-gratis/cor-verde-lineart-de-fundo-padrao_487879-550.jpg?w=900&t=st=1677875918~exp=1677876518~hmac=fa0f324cb7c4916ab21dca551183bed81d140a436f0869faf3488d588ae520fb");
  background-size: contain;
}

.backFace {
  background-color: white;
  transform: rotateY(180deg);
}

.card:hover {
  animation: selectecCard 0.5s forwards ease-in-out;
}

@keyframes selectecCard {
  100% {
    transform: rotateY(180deg);
  }
}
</style>