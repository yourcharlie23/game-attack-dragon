<template>
  <div class="confirm-box">
    <div class="backdrop"></div>
    <div class="box-container">
      <h3>{{this.message}}</h3>
      <div class="btn-grp">
        <button @click.prevent="$emit('onOkay')">Okay</button>
        <button @click.prevent="$emit('onCancel')" class="cancel">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ConfirmBox',
  props: {
    message: String,
    onOkay: Function,
    onCancel: Function
  },
  data () {
    return {
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.confirm-box {
    z-index: 2;
}
.box-container {
  position: absolute;
  left: 50%;
  top: 45%;
  transform: translate(-50%, -45%);
  width: 80%;
  background-color: #c7def1;
  border: 1px solid #81c1f5;
}
.backdrop {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
  background-color: rgba(249, 247, 247, 0.33);
}
.btn-grp {
  padding: 20px;
}
button {
  margin: 0 10px;
  padding: 10px;
  background-color: #81c1f5;
  color: white;
  font-size: 15px;
  font-weight: bold;
}
button.cancel {
  background-color: grey;
}
</style>
