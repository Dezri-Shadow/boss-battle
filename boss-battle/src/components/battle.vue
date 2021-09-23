<template>
  <div class="modal-backdrop">
    <div class="modal">
      <div class="bossSec">
        <img class="boss-battle" id="boss" :src="bossUrl"/>
        <h1> {{bossHP}}/{{bossAtk}}</h1>
      </div>

      <div class="heroSec">
        <img class="character-battle" id="hero" :src="heroUrl" />
        <h1> {{heroHP}}/{{heroMaxHP}}</h1>
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
        @click="close"
        aria-label="Close modal"
      >
        Close
      </button>
    </div>
  </div>

    
</template>


<script>
    export default {
        name: 'battle',

        props: {
          level: Number,
          isActive: Boolean
        },

        data () {
          return {
            bossHP: 10,
            bossAtk: 10,
            bossDef: 10,
            heroUrl: require( "@/assets/hero1.png" ),
            bossUrl: require( '@/assets/dragon.png' ),
            bossAction: 0,
            achPts: 100
          };
        },

        computed: {
          heroMaxHP: function () {
            return this.level
          },

          heroAtk: function () {
            return this.level
          },

          heroDef: function () {
            return this.level
          }
        },

        methods: {
            close() {
                this.$emit('close');
            },

            attack(){
              this.bossAction = Math.round(Math.random()*10); // Generate random action 
              if(this.bossAction == 0) {this.bossAction++;}// non-0 bossAction

              if (this.bossAction < 4){ //if bossAction is 1-3 (defend)
                this.heroHP = this.heroHP - this.bossDef;
                if (this.heroHP <= 0) {
                  this.defeat();
                }

              } else if (this.bossAction > 3 && this.bossAction < 8){ // if bossAction is 4-7 (attack)
                this.heroHP = this.heroHP - this.bossAtk;
                this.bossHP = this.bossHP - this.heroAtk;
                if (this.heroHP <= 0) {
                  this.defeat();
                } else if (this.bossHP <= 0) {
                  this.victory();
                } 

              } else { // if bossAction is 8-10 (magic)
                this.bossHP = this.bossHP - this.heroAtk;
                if (this.bossHP <= 0){
                  this.victory();
                }
              }

            },

            defend(){
              this.bossAction = Math.ceil(Math.random()*10); // Generate random action

              if (this.bossAction < 4){ //if bossAction is 1-3 (defend)
                console.log('Nothing Happens!');

              } else if (this.bossAction > 3 && this.bossAction < 8){ // if bossAction is 4-7 (attack)
                this.bossHP = this.bossHP - this.heroDef;
                if (this.bossHP <= 0) {
                  this.victory();
                }

              } else { // if bossAction is 8-10 (magic)
                this.heroHP = this.heroHP - this.bossAtk;
                if (this.heroHP <= 0) {
                  this.defeat();
                }

              }
            },

            magic(){
              this.bossAction = Math.ceil(Math.random()*10); // Generate random action

              if (this.bossAction < 4){ //if bossAction is 1-3 (defend)
                this.bossHP = this.bossHP - this.heroAtk;
                if (this.bossHP <= 0) {
                  this.victory();
                }

              } else if (this.bossAction > 3 && this.bossAction < 8){ // if bossAction is 4-7 (attack)
                this.heroHP = this.heroHP - this.bossAtk;
                if (this.heroHP <= 0) {
                  this.defeat();
                }

              } else { // if bossAction is 8-10 (magic)
                this.heroHP = this.heroHP - this.bossAtk;
                this.bossHP = this.bossHP - this.heroAtk;
                if ( this.bossHP <= 0){
                  this.victory();
                }else if (this.heroHP <= 0){
                  this.defeat();
                }

              }
            },

            victory(){
              this.achPts = this.achPts - this.level;
              alert("YOU WIN! \r\n " + this.achPts + " Achievement Points");
              this.close();
            },

            defeat(){
              alert("GAME OVER!");
              this.close();
            }
        }
    }
    
    

</script>

<style scoped>
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