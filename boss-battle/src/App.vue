<template>
  <div id="app">

    <button
      type="button"
      class="btn"
      @click="showCharacterSheet"
    >
      Open Character Sheet
    </button>
    <img @click="moveRight" :style="cssVars" class="character" id="hero" :src="heroUrl" />
    <img class="boss" id="boss" :src="bossUrl"/>

    <div v-show="isCharacterSheetVisible">
      <div class="modal-backdrop">
        <div class="modal">
          <div class="header">
            <h1> {{characterName}}'s Character Sheet </h1>
            <button
              type="button"
              class="btn-green"
              aria-label="Close modal"
              @click="closeCharacterSheet"
            >
              Close
            </button>
          </div>

          <div class="body">
            <div class='characterClass'><h2>Class : {{role}}</h2> Level : {{level}}</div>
            <div class="stats">
              <div>P.Atk : {{level}}</div>
              <div>M.Atk : {{level}}</div>
              <div>P.Def : {{level}}</div>
              <div>M.Def : {{level}}</div>
              <div>HP : {{level}}</div>
            </div>
          </div>

          <div class='level-btns'>
            <button 
              type="button"
              class="btn-level"
              v-if='level < 99'
              @click='levelUp'
            >
              Level Up
            </button>

            <button
              type="button"
              class="btn-level"
              v-if='level > 1'
              @click='levelDown'
            >
              Level Down
            </button>
          </div>
        </div>
      </div>
    </div>

    <div v-show="fight">
      <div class="modal-backdrop-fight">
        <div class="modal-fight">
          <div class="bossSec">
            <img class="boss-battle" id="boss" :src="bossUrl"/>
            <h1> {{bossHP}}/{{bossAtk}}</h1>
          </div>

          <div class="heroSec">
            <img class="character-battle" id="hero" :src="heroUrl" />
            <h1> {{heroCurrentHP}}/{{heroMaxHP}}</h1>
          </div>

          <div class="actionSec">
            <button
              type="button"
              class="attack-btn"
              @click="attack"
            >
              ATTACK 
            </button>
        
            <button
              type="button"
              class="attack-btn"
              @click="defend"
            >
              DEFEND  
            </button>

            <button
              type="button"
              class="attack-btn"
              @click="magic"
            >
              CAST MAGIC 
            </button>
          </div>

          <button
            type="button"
            class="close-btn"
            @click="closeBattle"
            aria-label="Close modal"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',

  data () {
      return {
        isCharacterSheetVisible: false,
        fight: false,
        pos: -616,
        heroUrl: require( "./assets/hero1.png" ),
        bossUrl: require( './assets/dragon.png' ),
        characterName: 'Our Hero',
        role: 'Hero',
        level: 1,
        bossHP: 10,
        bossAtk: 10,
        bossDef: 10,
        bossAction: 0,
        achPts: 100,
        heroMaxHP: 1,
        heroAtk: 1,
        heroDef: 1,
        heroCurrentHP: 1
      };
  },
  computed: {
    cssVars() {
      return {
        '--pos': this.pos + 'px'
      }
    }
  },
  methods: {
      showCharacterSheet() {
        this.isCharacterSheetVisible = true;
      },
      closeCharacterSheet() {
        this.isCharacterSheetVisible = false;
      },
      closeBattle() {
        this.fight = false;
      },
      levelUp() {
        this.level++;
        this.heroMaxHP++;
        this.heroAtk++;
        this.heroDef++;
        this.heroCurrentHP++;
      },
      levelDown() {
        this.level--;
        this.heroMaxHP--;
        this.heroAtk--;
        this.heroDef--;
        this.heroCurrentHP--;
      },
      moveRight() {
        this.pos += 84; 
        if(this.pos == 560){
          this.fight = true;
        }
      },

      checkWhoWon(){
        if(this.bossHP <= 0){
          this.victory();
        } else if (this.heroCurrentHP <= 0) { 
          this.defeat();
        } 
      },

      attack(){
        this.bossAction = Math.round(Math.random()*10); // Generate random action 
        if(this.bossAction == 0) {this.bossAction++;}// non-0 bossAction

        if (this.bossAction < 4){ //if bossAction is 1-3 (defend)
          this.heroCurrentHP = this.heroCurrentHP - this.bossDef;
          this.checkWhoWon();

        } else if (this.bossAction > 3 && this.bossAction < 8){ // if bossAction is 4-7 (attack)
          this.heroCurrentHP = this.heroCurrentHP - this.bossAtk;
          this.bossHP = this.bossHP - this.heroAtk;
          this.checkWhoWon();

        } else { // if bossAction is 8-10 (magic)
          this.bossHP = this.bossHP - this.heroAtk;
          this.checkWhoWon();
        }

      },

            defend(){
              this.bossAction = Math.ceil(Math.random()*10); // Generate random action

              if (this.bossAction < 4){ //if bossAction is 1-3 (defend)
                console.log('Nothing Happens!');

              } else if (this.bossAction > 3 && this.bossAction < 8){ // if bossAction is 4-7 (attack)
                this.bossHP = this.bossHP - this.heroDef;
                this.checkWhoWon();

              } else { // if bossAction is 8-10 (magic)
                this.heroCurrentHP = this.heroCurrentHP - this.bossAtk;
                this.checkWhoWon();

              }
            },

            magic(){
              this.bossAction = Math.ceil(Math.random()*10); // Generate random action

              if (this.bossAction < 4){ //if bossAction is 1-3 (defend)
                this.bossHP = this.bossHP - this.heroAtk;
                this.checkWhoWon();

              } else if (this.bossAction > 3 && this.bossAction < 8){ // if bossAction is 4-7 (attack)
                this.heroCurrentHP = this.heroCurrentHP - this.bossAtk;
                this.checkWhoWon();

              } else { // if bossAction is 8-10 (magic)
                this.heroCurrentHP = this.heroCurrentHP - this.bossAtk;
                this.bossHP = this.bossHP - this.heroAtk;
                this.checkWhoWon();

              }
            },

            victory(){
              this.achPts = this.achPts - this.level;
              alert("YOU WIN! \r\n " + this.achPts + " Achievement Points");
              this.closeBattle();
            },

            defeat(){
              alert("GAME OVER!");
              this.closeBattle();
            }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 60px;
  margin-bottom: 60px;
  background-image: url('./assets/dungeon.jpg');
  min-height: 1000px;
  background-repeat: no-repeat;
  max-width: 1500px;
}

.btn {
    padding: 10px;
    background-color: #00599a;
    margin: 10px;
    border: solid 2px #000;
    border-radius: 10%;
    color: #fff;
    position: absolute;

}
.btn:hover{
  background-color: #fff;
  color: #00599a;
  border-color: #00599a;
  font-weight: bold;
}
.btn:focus{
  outline: -webkit-focus-ring-color auto 0px;
}

.character {
  position: relative;
  top: 510px;
  left: var(--pos);
}

.boss {
  position: relative;
  top: 516px;
  right: -520px;
}

/* character sheet */

.modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }

  .header {
    padding: 0px 20px;
    text-align: center;
  }

  .stats div {
    max-width: 40%;
    margin: 10px;
    padding: 10px;
    border: 2px solid #00599a;
    border-radius: 10%;
    display: inline-block;
    color: #00599a;
  }

  .stats {
    width: 98%;
    margin-bottom: 10px;
  }

  .btn-level {
    padding: 10px;
    background-color: #00599a;
    margin: 10px;
    border: solid 2px #fff;
    border-radius: 10%;
    color: #fff;
  }

  .btn-level:hover{
    background-color: #fff;
    color: #00599a;
    border-color: #00599a;
    font-weight: bold;
  }

  .btn-level:focus{
    outline: -webkit-focus-ring-color auto 0px;
  }

  .characterClass {
    margin-left: 10px;
  }

  /* battle section */

  .modal-backdrop-fight {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal-fight {
    background: #000000;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    width: 99%;
    height: 600px;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .attack-btn {
    border: 4px solid #eeeeee;
    border-radius: 15%;
    color: #eeeeee;
    background-color: #000000;
  }

  .heroSec {
    width: 30%;
    border: 4px solid #eeeeee;
    border-radius: 15%;
    color: #eeeeee;
  }

  .bossSec{
    width: 50%;
    border: 4px solid #eeeeee;
    border-radius: 15%;
    color: #eeeeee;
    margin-right: 1px;
  }

  .actionSec {
    width: 90%;
    border: 4px solid #eeeeee;
    border-radius: 15%;
    padding-top: 20px;
    padding-bottom: 20px;
  }
  
  .character-battle {
    -webkit-transform: scaleX(-1);
    transform: scaleX(-1);
    padding-top: 60px;
  }

  .boss-battle {
    -webkit-transform: scaleX(-1);
    transform: scaleX(-1);
    padding-top: 50px;
  }
</style>
