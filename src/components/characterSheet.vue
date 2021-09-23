<template>
  <div class="modal-backdrop">
    <div class="modal">
      <div class="header">
        <h1> {{characterName}}'s Character Sheet </h1>
      </div>

      <div class="body">
        <div class='characterClass'><h2>Class : {{role}}</h2> Level : {{lvl}}</div>
        <div class="stats">
          <div>P.Atk : {{physAtk}}</div>
          <div>M.Atk : {{magAtk}}</div>
          <div>P.Def : {{physDef}}</div>
          <div>M.Def : {{magDef}}</div>
          <div>HP : {{hp}}</div>
        </div>
      </div>

      <div class='level-btns'>
        <button 
          type="button"
          class="btn-level"
          v-if='lvl < 99'
          @click='levelUp'
        >
          Level Up
        </button>

        <button
          type="button"
          class="btn-level"
          v-if='lvl > 1'
          @click='levelDown'
        >
          Level Down
        </button>
      </div>
       <button
          type="button"
          class="btn-green"
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
  name: 'characterSheet',

  data() {
    return {
      characterName: 'Our Hero',
      physAtk: 1,
      magAtk: 1,
      physDef: 1,
      magDef: 1,
      hp: 1,
      role: 'Hero',
      lvl: 1
    }
  },

  methods: {
      close() {
        this.$emit('close');
      },

      levelUp() {
        this.physAtk += 1;
        this.magAtk += 1;
        this.physDef += 1;
        this.magDef += 1;
        this.hp += 1;
        this.lvl += 1;
        this.$emit('levelUp');
      },

      levelDown() {
        this.physAtk -= 1;
        this.magAtk -= 1;
        this.physDef -= 1;
        this.magDef -= 1;
        this.hp -= 1;
        this.level -= 1;
        this.$emit('levelDown');
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


  .body {
    text-align: left;
  }

</style>
