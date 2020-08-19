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

    <characterSheet
      v-show="isCharacterSheetVisible"
      @close="closeCharacterSheet"
      @levelUp="levelUp"
      @levelDown="levelDown"
    ></characterSheet>

    <battle 
      v-show="fight"
      @close="closeBattle"
      :level='level'
      ></battle>
  </div>

</template>

<script>
import characterSheet from './components/characterSheet.vue';
import battle from './components/battle.vue';

export default {
  name: 'App',
  components: {
    characterSheet,
    battle
  },

  data () {
      return {
        isCharacterSheetVisible: false,
        fight: false,
        pos: -616,
        heroUrl: require( "./assets/hero1.png" ),
        bossUrl: require( './assets/dragon.png' ),
        level: 1
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
        level++;
      },
      levelDown() {
        level--;
      },
      moveRight() {
        this.pos += 84; 
        if(this.pos == 560){
          this.fight = true;
        }
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
</style>
