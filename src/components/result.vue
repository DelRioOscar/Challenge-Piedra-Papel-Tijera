<template>
  <div>
    <section class="game__result">
      <div class="game__you">
        <p>Elegiste {{yourSelected}}</p>
        <div :class="[participant.you, { 'game--winner': final == 'Has Ganado' }]"></div>
      </div>
      <div class="game__final">
        <p class="game__title">{{final}}</p>
        <button class="btn btn__primary" @click="again()">Jugar de nuevo</button>
      </div>
      <div class="game__rival">
        <p>Tu rival eligió {{rivalSelected}}</p>
        <div :class="[participant.rival, { 'game--winner': final == 'Has Perdido' }]"></div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "Result",
  methods: {
    again() {
      this.$emit("gameAgain", false);
    },
  },
  props: {
    winner: String,
    rivalSelected: String,
    yourSelected: String,
    final: String,
    participant: Object,
  },
};
</script>

<style>
</style>