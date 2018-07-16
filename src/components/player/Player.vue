<template>
  <div class="player">
    <h2>{{ this.name }}</h2>
    <health-bar :health="health" />
    <console-control 
      v-if="this.isStarted && !this.isRobot" 
      :onAttack="attack" 
      :onBlast="blast"
      :onHeal="heal" 
      :onGiveUp="giveUp" />
  </div>
</template>

<script>
import healthBar from '@/components/player/healthBar'
import consoleControl from '@/components/player/consoleControl'
import _debounce from 'lodash/debounce';

export default {
  name: 'Player',

  props: {
    'pid': String, 
    'name': String, 
    'health': Number,
    'isRobot': Boolean,
    'isStarted': Boolean,
    'onAttack': Function, 
    'onBlast': Function, 
    'onHealth': Function, 
    'onGiveUp': Function, 
  },

  components: {
    healthBar,
    consoleControl
  },


  data () {
    return {
      blastPlus: 5,
      randomMin: 1,
      randomMax: 10,
      robotResponseTime: 500
    }
  },

  watch: {
    'health': function (val, oldVal) {
      // for robots
      if (val === 100) return
      const hasMoreChance = oldVal - this.blastPlus >= val
      const self = this
      if (this.isRobot && this.health > 0) {
        setTimeout(function() {
          if (self.randomAttackType(hasMoreChance) === 2) {
            self.blast()
          } else {
            self.attack()
          }
        }, self.robotResponseTime)
      }
    }
  },

  computed: {
    
  },

  methods: {
    attack: _debounce(function () {
      this.$emit('onAttack', {
        pid: this.pid,
        damage: this.randomValue()
      })
    }, 500, {'leading': true, 'trailing': false} ),
    blast: _debounce(function () {
      this.$emit('onBlast', {
        pid: this.pid,
        damage: this.randomValue() + this.blastPlus,
        type: 'blast'
      })
    }, 500, {'leading': true, 'trailing': false} ),
    heal: _debounce(function () {
      this.$emit('onHeal', {
        pid: this.pid,
        heal: this.randomValue()
      })
    }, 500, {'leading': true, 'trailing': false} ),
    giveUp: _debounce(function () {
      this.$emit('onGiveUp', {
        pid: this.pid
      })
    }, 500, {'leading': true, 'trailing': false} ),
    randomAttackType (hasMoreChance) {
      const chance = hasMoreChance ? 5 : 3
      const attackType = Math.floor(Math.random() * chance) + 1
      // Attack type 1=normal, 2=blast
      return attackType > 2 ? 2 : 1
    },
    randomValue () {
      return Math.floor(Math.random() * this.randomMax) + this.randomMin;
    }
  }


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.player {
  display: inline-block;
  padding: 20px;
  vertical-align: top;
}
</style>
