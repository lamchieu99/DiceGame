<template>
	<div id="app">
		<div class="wrapper clearfix">
            <players 
                    v-bind:isWinner = "isWinner"
                    v-bind:scoresPlayer = "scoresPlayer"
                    v-bind:activePlayer = "activePlayer"
                    v-bind:currentScore = "currentScore"
            />
            <controls 
                v-bind:isPlaying = "isPlaying"
                v-bind:finalScore = "finalScore"
                v-on:handleChangeFinalScore = "handleChangeFinalScore"
                v-on:handleHoldScore = "handleHoldScore"
                v-on:handleNewGame = "handleNewGame"
                v-on:handleRollDice = "handleRollDice"
            />
            <dices 
                v-bind:dices = "dices"
            />
            <popup-rule 
                v-bind:isOpenPopup = "isOpenPopup"
                v-on:handleConfirm = "handleConfirm"
            />
        </div>
	</div>
</template>

<script>

import Players from './components/Players'
import Controls from './components/Controls'
import Dices from'./components/Dices'
import PopupRule from './components/PopupRule'
export default {
	name: 'app',
	data () {
		return {
            scoresPlayer: [0, 0],
            activePlayer: 0,
            currentScore: 30,
            isPlaying: false,
            isOpenPopup: false,
            dices: [2, 6],
            finalScore: 5
		}
	},
	components: {
        Players,
        Controls,
        Dices,
        PopupRule
    },
    computed: {
        isWinner() {
            let {scoresPlayer, finalScore} = this;
            if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
                this.isPlaying = false;
                return true;
            }
            return false;
        }
    },
    methods: {
        handleNewGame() {
            this.isOpenPopup = true;
        },
        nextPlayer() {
            this.activePlayer = this.activePlayer === 0 ? 1 : 0;
            this.currentScore = 0;
        },
        handleConfirm() {
            this.isPlaying = true;
            this.activePlayer = 0;
            this.scoresPlayer = [0, 0];
            this.currentScore = 0;
            this.isOpenPopup = false;
            this.dices = [1, 1];
        },
        handleRollDice() {
            console.log('Handle App.vue');
            if (this.isPlaying) {
                var dice1 = Math.floor(Math.random() * 6) + 1;
                var dice2 = Math.floor(Math.random() * 6) + 1;
                this.dices = [dice1, dice2];
                console.log(dice1, dice2);

                if(dice1 === 1 || dice2 ===1) {
                    
                    setTimeout(() => {
                        alert(`Nguoi choi Player ${this.activePlayer + 1} da quay trung so 1. Rat tiec!`);
                    }, 10)
                    this.nextPlayer();
                }
                else {
                    this.currentScore  = this.currentScore + dice1 + dice2;
                }
            }
            else {
                alert ('Vui long nhan vao nut NewGame')
            }
        },
        handleHoldScore() {
            if(this.isPlaying) {
                let { scoresPlayer, currentScore, activePlayer } = this;
                let oldScore = scoresPlayer[activePlayer];
                // let cloneScore = [...scoresPlayer];
                //     cloneScore[activePlayer] = oldScore + currentScore;
                // this.scoresPlayer = cloneScore;
                // console.log(scoresPlayer);
                this.$set(this.scoresPlayer, this.activePlayer, oldScore + currentScore)
                if(!this.isWinner) {
                    this.nextPlayer();
                }
            }
            else {

            }
        },
        handleChangeFinalScore(e){
            console.log(e.target.value);
            if (isNaN(number)) {
                this.finalScore = '';
            }
            else {
                this.finalScore = number;
            }
        }
    }
}
</script>

<style>
	* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(/public/assets/back.jpg);
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}

</style>