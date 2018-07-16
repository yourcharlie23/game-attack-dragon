<template>
	<div>
    <div>
  	  <Player v-for='(player, index) in players' 
  	  	:key="index" 
  	  	:name="player.name" 
  	  	:pid="player.pid" 
  	  	:isRobot="player.isRobot" 
  	  	:health="player.health" 
  	  	:isStarted="isStarted"
  	  	@onAttack="attack" 
  	  	@onBlast="blast" 
  	  	@onHeal="heal" 
  	  	@onGiveUp="giveUp" 
  	  	class="game__player" />
    </div>
	  <button v-if="!this.isStarted" class="start" @click.prevent='startGame'>START</button>
	  <Commentary :comments="comments" :max="maxComment" class="game__commentary" />
	  <ConfirmBox v-if="isOpenConfirmBox" :message="popOverMsg" @onOkay="startGame" @onCancel="hideConfirmBox"/>
	</div>
</template>

<script>
import Player from '@/components/player/Player'
import Commentary from '@/components/Commentary'
import ConfirmBox from '@/components/ConfirmBox'

export default {
  name: 'Game',
  components: {
    Player,
    Commentary,
    ConfirmBox
  },

  mounted () {
  	this.resetGame()
  },

  data () {
    return {
    	maxComment: 10,
    	isStarted: false,
    	isGameOver: false,
    	isOpenConfirmBox: false,
    	popOverMsg: '',
      players: [],
      comments: [],
    }
  },

  methods: {
  	startGame: function () {
  		this.resetGame()
  		this.isStarted = true
  		this.isGameOver = false
  		this.isOpenConfirmBox = false
  	},
  	resetGame: function () {
  		this.players = [
      	{
      		name: 'Player 1',
      		pid: 'player1',
      		isRobot: false,
      		health: 100
      	},
      	{
      		name: 'Dragon',
      		pid: 'dragon',
      		isRobot: true,
      		health: 100
      	},
      ]
      this.comments = []
  	},
  	hideConfirmBox: function() {
  		this.isOpenConfirmBox = false
  	},
  	processGameOver: function (playerIndex) {
  		this.popOverMsg = `${this.players[playerIndex].name} Win! Do you want a re-match?`
			this.isOpenConfirmBox = true
			this.isStarted = false
			this.addComment(`${this.players[playerIndex].name} Win!`)
  	},
  	attack: function (params) {
  		let index = 0
  		let otherIndex = 0
  		const self = this
  		this.players.forEach(function(player, idx) {
  			if (player.pid !== params.pid) {
  				otherIndex = idx
  				const newHealth = (player.health - params.damage)
  				if (newHealth <= 0) {
  					//game over
  					player.health = 0
						self.isGameOver = true
  				} else {
  					player.health = newHealth
  				}
  			} else {
  				index = idx
  			}
  		})

  		this.addComment(`${this.players[index].name} ${params.type === 'blast' ? 'blasts' : 'attacks'} ${this.players[otherIndex].name} for ${params.damage}`)
  		if (this.isGameOver) {
  			self.processGameOver(index)
  		}
	  },
	  blast: function (params) {
  		this.attack(params)
	  },
	  heal: function (params) {
	  	let index = 0
  		this.players.some(function(player, idx) {
	  		if (player.pid === params.pid) {
	  			index = idx
					const newHealth = (player.health + params.heal)
					player.health = newHealth > 100 ? 100 : newHealth
				} else {
					player.health--;
				}
			})

			this.addComment(`${this.players[index].name} heals for ${params.heal}`)
	  },
	  giveUp: function (params) {
	  	let index = 0
	  	let otherIndex = 0
	  	const self = this
  		this.players.forEach(function(player, idx) {
	  		if (player.pid === params.pid) {
	  			index = idx
  				player.health = 0
  				self.isGameOver = true
  				return true
  			} else {
  				otherIndex = idx
  			}
			})

			this.addComment(`${this.players[index].name} gave up.`)
			self.processGameOver(otherIndex)
	  },
	  addComment: function (comment) {
	  	this.comments.unshift(comment)
	  }
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game__player {
	width: 40%;
	min-width: 230px;
}
.game__commentary {
	width: 60%;
	min-width: 250px;
	position: relative;
	left: 50%;
	transform: translateX(-50%);
	margin-top: 50px;
}
.start {
	margin-top: 50px;
	padding: 10px;
	background-color: green;
	color: white;
	font-size: 15px;
	font-weight: bold;
}
</style>
