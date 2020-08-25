<template>
  <div>
    <section class="indicators">
      <div class="indicators__types">
        <h2 class="indicators__type">Piedra</h2>
        <h2 class="indicators__type">Papel</h2>
        <h2 class="indicators__type">Tijera</h2>
      </div>
      <div class="indicators__scores">
        <span class="indicators__title">Puntaje</span>
        <h1 class="indicators__score">{{score}}</h1>
      </div>
    </section>

    <result
      v-if="isResult"
      v-on:gameAgain="gameAgain($event)"
      :rivalSelected="enemySelected"
      :yourSelected="typeSelected"
      :final="winner"
    ></result>
    <template v-if="!isResult">
      <section class="game__selected" v-if="typeSelected">
        <h1 class="game__title">Has elegido {{typeSelected}}</h1>
      </section>

      <section class="game__selected" v-if="!typeSelected">
        <h1 class="game__title">Selecciona Piedra, Papel y Tijera</h1>
      </section>

      <section class="game" v-bind:class="{'game--process': isProcessing}">
        <div @click="onSelected('Papel')" class="game__paper"></div>
        <div @click="onSelected('Tijera')" class="game__scissor"></div>
        <div @click="onSelected('Piedra')" class="game__rock"></div>
      </section>
    </template>
  </div>
</template>
<script>
import Result from "@/components/result";

export default {
  name: "Home",
  components: {
    Result,
  },
  data() {
    return {
      isResult: false,
      isProcessing: false,
      winner: "",
      typeSelected: "",
      score: 0,
      max: 2,
      min: 0,
      enemySelected: "",
      possibilities: ["Papel", "Tijera", "Piedra"],
      isLoading: false
    };
  },
  methods: {
    onSelected(name) {
      this.typeSelected = name;
      this.isProcessing = true;

      if (!this.isLoading) {
        this.isLoading = true;
        setTimeout(() => {
          this.isProcessing = false;
          this.isResult = true;

          const enemy = Math.floor(
            Math.random() * (this.max - this.min + 1) + this.min
          );
          this.enemySelected = this.possibilities[enemy];
          this.validate();
        }, 2000);
      }
    },
    gameAgain(again) {
      this.isResult = again;
      this.typeSelected = "";
    },
    validate() {
      if (this.enemySelected == this.typeSelected) {
        this.winner = "Empate";
      } else {
        if (this.enemySelected == "Papel" && this.typeSelected == "Tijera") {
          this.winner = "Has Ganado";
          this.score++;
        } else if (
          this.enemySelected == "Tijera" &&
          this.typeSelected == "Piedra"
        ) {
          this.winner = "Has Ganado";
          this.score++;
        } else if (
          this.enemySelected == "Papel" &&
          this.typeSelected == "Tijera"
        ) {
          this.winner = "Has Ganado";
          this.score++;
        } else {
          this.winner = "Has Perdido";
          if (this.score > 0) {
            this.score--;
          }
        }
      }
      this.isLoading = false;
    },
  },
};
</script>
