<template>
  <div class="health-bar" v-bind:class="classObject">
    <div>{{this.health}} %</div>
    <div class="per-bar" :style="{width: this.health + '%'}"></div>
  </div>
</template>

<script>
export default {
  name: 'health-bar',
  props: {
    health: Number
  },
  data () {
    return {
    }
  },
  computed: {
    classObject: function () {
      return {
        full: this.health === 100,
        empty: this.health <= 0,
        warning: this.health < 100 && this.health > 0
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.health-bar {
  width: 100%;
  color: white;
  border: 1px solid #676565;
  position: relative;
}
.health-bar.full {
  background-color: green;
  border-color: #014601;
}
.health-bar.warning {
  background-color: orange;
  border-color: #b17302;
}
.health-bar.empty {
  background-color: red;
  border-color: #a00202;
}
.per-bar {
  height: 100%;
  background-color: #ffeb3b;
  position: absolute;
  top: 0;
  right: 0;
  opacity: 0.4;
}
</style>
