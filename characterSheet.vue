<template>
  <div class="modal-backdrop">
    <div class="modal">
      <div class="header">
        <h1> {{characterName}}'s Character Sheet </h1>
      </div>

      <div class="body">
        <div class='characterClass'><h2>Class : {{role}}</h2> Level : {{level}}</div>
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
      level: 1
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
        this.level++;
        this.$emit('levelUp');
      },

      levelDown() {
        this.physAtk -= 1;
        this.magAtk -= 1;
        this.physDef -= 1;
        this.magDef -= 1;
        this.hp -= 1;
        this.level--;
        this.$emit('levelDown');
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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

  .body {
    text-align: left;
  }

</style>
