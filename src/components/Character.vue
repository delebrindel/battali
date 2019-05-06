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
      minHeal: Number,
      maxHeal: Number,
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
      CombatFlow.$on('heal', this.onHeal);
      CombatFlow.$on('damage', this.onDamaged);
    },
    methods: {
      calculateDamage: function (min, max) {
        return Math.max(Math.floor(Math.random() * max + 1), min);
      },
      onAttacked: function (type) {
        if(type === "player" && this.char ==="player"){
          this.normalAttack(type)
        }
      },
      onHeal: function(type){
        if(this.char === type){
          let damage = this.calculateDamage(this.minHeal, this.maxHeal)*1.5;
          if(this.hp + damage < this.maxHp){
            this.hp += damage;
          }
          else{
            damage = this.maxHp - this.hp;
            this.hp = this.maxHp;
          }
            CombatFlow.$emit('add-to-log',{origin: 'player', message: `Player healed for ${damage} damage`})
            damage = this.calculateDamage(this.minDamage, this.maxDamage);
            CombatFlow.$emit('damage',['player', damage]);
        }

      },
      onDamaged: function (data) {
        let type = data[0];
        let damage = data[1];
        if(this.char === type){
          if(this.hp - damage > 0){
            this.hp -= damage;
            if( (this.rage + damage * 2) < this.maxRage){
              this.rage += damage * 2;
            }
            else{
              this.rage = this.maxRage;
            }
            if(this.rage === this.maxRage){
              CombatFlow.$emit('has-max-rage',[this.char])
            }
          }
          else{
            this.hp = 0;
          }
          if(type === "monster"){
            CombatFlow.$emit('add-to-log',{origin: 'player', message: `Player attacked monster, dealing ${damage} damage`})
            damage = this.calculateDamage(this.minDamage, this.maxDamage);
            CombatFlow.$emit('damage',['player', damage])
          }
          else{
            CombatFlow.$emit('add-to-log',{origin: 'monster', message: `Monster attacked player, dealing ${damage} damage`})
          }
        }
      },
      Heal: function(type){
        var damage = this.calculateDamage(this.minDamage, this.maxDamage)*1.5;
        if(type === this.char && this.char ==="player"){
          CombatFlow.$emit('heal', ['player', damage]);
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