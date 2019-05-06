<template>
  <div class="character" :class="char">
    Health
    <div class="hp-bar">
      <div class="hp-bar--inner" :class="'hp-bar--inner--'+char" :style="{ width: (hp/parseInt(maxHp))*100+'%'}">
        {{hp}} / {{maxHp}}
      </div>
    </div>
    Rage
    <div class="rage-bar">
      <div class="rage-bar--inner" :class="'rage-bar--inner--'+char"
        :style="{ width: (rage/parseInt(maxRage))*100+'%'}">
        {{rage}} / {{maxRage}}
      </div>
    </div>
  </div>
</template>

<script>
  import { CombatFlow } from "../main.js";
  export default {
    name: 'Character',
    props: {
      char: String,
      maxHp: Number,
      maxRage: Number,
      minDamage: Number,
      maxDamage: Number
    },
    data: () => {
      return {
        hp: 0,
        rage: 0
      }
    },
    mounted() {
      this.hp = this.maxHp;
      this.normalAttack();
    },
    created() {
      CombatFlow.$on('attack', this.onAttacked);
      CombatFlow.$on('damage', this.onDamaged);
    },
    methods: {
      calculateDamage: function (min, max) {
        return Math.max(Math.floor(Math.random() * max + 1), min);
      },
      onAttacked: function (type) {
        if(type === "monster" && this.char ==="monster"){
          console.log(type);
        }
        else if(type === "player" && this.char ==="player"){
          this.normalAttack(type)
        }
      },
      onDamaged: function (data) {
        let type = data[0];
        let damage = data[1];
        if(type === type && this.char ===type){
          if(this.hp - damage > 0){
            this.hp -= damage;
            this.rage = this.rage + damage * 2;
          }
          else{
            this.hp = 0;
          }
          if(type === "monster"){
            console.log(`Monster was attacked for ${damage}`);
            var retaliate = this.calculateDamage(this.minDamage, this.maxDamage);
            CombatFlow.$emit('damage',['player', retaliate])
          }
          else{
            console.log(`Player was attacked for ${damage}`);
          }
        }
      },
      normalAttack: function(type){
        var damage = this.calculateDamage(this.minDamage, this.maxDamage);
        if(type === "player" && this.char ==="player"){
          CombatFlow.$emit('damage', ['monster', damage]);
        }
        else if(type === "monster" && this.char ==="monster"){
          CombatFlow.$emit('damage', ['player', damage]);
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "@/assets/scss/_theme.scss";

  .hp-bar,
  .rage-bar {
    width: 60%;
    border: 2px solid;
    margin: 0 auto;
    height: 2rem;
    line-height: 1rem;
    border-radius: 1rem;
    overflow: hidden;

    .hp-bar--inner,
    .rage-bar--inner {
      height: 100%;
      color: #fff;
      font-weight: bold;
      -webkit-transition: width 1s ease-in-out;
      -moz-transition: width 1s ease-in-out;
      -o-transition: width 1s ease-in-out;
      transition: width 1s ease-in-out;
    }

    .hp-bar--inner--player {
      background: $player-color--hp-bar;
    }

    .hp-bar--inner--monster {
      background: $monster-color--hp-bar;
    }

    .rage-bar--inner--player {
      background: $player-color--rage-bar;
    }

    .rage-bar--inner--monster {
      background: $monster-color--rage-bar;
    }
  }
</style>