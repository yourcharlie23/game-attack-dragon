<template>
  <div class="commentary" >
    <strong>Game Commentary</strong>
    <div class="container" v-bind:class="{hasNewComment: hasNewComment}">
      <div v-for='(comment, index) in listComments' :key="index" class="row" v-bind:class="{odd: index % 2 !== 0}">
        {{comment}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Commentary',

  props: {
    comments: Array,
    max: Number
  },

  data () {
    return {
      hasNewComment: false
    }
  },

  watch: {
    'comments': function (val, oldVal) {
      this.hasNewComment = true
      const self = this
      setTimeout(function() {
        self.hasNewComment = false
      }, 300)
    }
  },

  computed: {
    listComments: function () {
      const max = this.max
      return this.comments.filter(function (comment, idx) {
        return idx < max
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.commentary {
  background-color: white;
  color: black;
  border: 2px solid #81c1f5;
}
.container {
  max-height: 190px;
  overflow: auto;
}
.hasNewComment .row:first-child {
  opacity: .1;
}
.row {
  border-top: 1px solid #81c1f5;
  transition: opacity 0.5s;
  opacity: 1;
}
.odd {
  background-color: #c5e0f7;
}
</style>
