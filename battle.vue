<template>
  <div class="modal-backdrop">
    <div class="modal">
      <div class="bossSec">
        <img src="" />
      </div>

      <div class="heroSec">
        <img src="" />
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

        props: 'level',

        data () {
          return {
            heroHP: level,
            heroAtk: level,
            heroDef: level,
            bossHP: 10,
            bossAtk: 10,
            bossDef: 10,

          };
        },

        methods: {
            close() {
                this.$emit('close');
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
    flex-direction: column;
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
  }

  .heroSec {
    width: 30%;
  }

  .bossSec{
    width: 60%;
  }

  .actionSec {
    width: 90%;
  }



</style>