<template>
  <div class="console-control">
    <button @click.prevent='onAttack'>Attack</button>
    <button @click.prevent='onBlast'>Blast</button>
    <button @click.prevent='onHeal'>Heal</button>
    <button @click.prevent='onGiveUp'>Give Up</button>
  </div>
</template>

<script>
export default {
  name: 'console-control',
  props: {
    onAttack: Function,
    onBlast: Function,
    onHeal: Function,
    onGiveUp: Function
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.console-control {
  padding: 10px;
  border: 1px solid #81c1f5;
}
button {
  background-color: #81c1f5;
}
</style>
