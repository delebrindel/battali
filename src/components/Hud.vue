<template>
  <div id="hud-inner">
    <div class="hud-inner--icons">
      <div class="hud-inner--icons--attack hud-icon" @click="attack">
        <span>Attack</span>
        <img src="@/assets/icons/hud/player/attack.svg" />
      </div>
      <div class="hud-inner--icons--heal hud-icon" @click="heal">
        <span>Heal</span>
        <img src="@/assets/icons/hud/player/remedy.svg" />
      </div>
      <div class="hud-inner--icons--special hud-icon" :class="{'hud-icon--disabled' : rage != 100}">
        <span>Special</span>
        <img src="@/assets/icons/hud/player/ember.svg" />
      </div>
      <div class="hud-inner--icons--run hud-icon">
        <span>Escape</span>
        <img src="@/assets/icons/hud/player/run.svg" />
      </div>
    </div>
    <div class="hud-inner--logs">
      <div v-for="(log, index) in logs" :key="index" :class="'hud-inner--logs--'+log.origin">
        <template v-if="log.origin == 'player'">
          <img src="@/assets/icons/hud/player.svg" alt="Player:">
          <img src="@/assets/icons/hud/player/heal.svg" v-if="log.action === 'heal'" alt="heal">
          <img src="@/assets/icons/hud/player/slash.svg" v-if="log.action === 'attack'" alt="attack">
          <img src="@/assets/icons/hud/player/special.svg" v-if="log.action === 'special'" alt="special attack">
        </template>
        <template v-else>
        <img src="@/assets/icons/hud/monster.svg" alt="Monster:">
          <img src="@/assets/icons/hud/monster/heal.svg" v-if="log.action === 'heal'" alt="heal">
          <img src="@/assets/icons/hud/monster/claw.svg" v-if="log.action === 'attack'" alt="attack">
          <img src="@/assets/icons/hud/monster/special.svg" v-if="log.action === 'special'" alt="special attack">
        </template>
        {{log.message}}
      </div>
    </div>
  </div>
</template>

<script>
  import {
    CombatFlow
  } from "../main.js";
  export default {
    name: 'Hud',
    data: () => {
      return {
        rage: 0,
        logs: []
      }
    },
    methods: {
      attack: function () {
        CombatFlow.$emit('attack', 'player');
      },
      heal: function(){
        CombatFlow.$emit('heal', 'player');
      },
      onAddToLog: function(data){
        this.logs.push(data);
      }
    },
    created() {
      CombatFlow.$on('add-to-log', this.onAddToLog);
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "@/assets/scss/_theme.scss";

  .hud-inner--icons {
    display: grid;
    grid-template-columns: 2fr repeat(4, 1fr) 2fr;
    font-size: 1.5rem;
    text-align: center;
  }

  .hud-icon {
    span {
      display: block;
    }

    img {
      margin: 0.25rem auto;
      display: block;
      width: 60%;
    }
  }

  .hud-icon--disabled {
    opacity: 0.5;
  }
  .hud-inner--logs{
    margin-top: 1rem; 
    font-weight: bold;
    font-size: 1rem;
    line-height: 1.5rem;
    display: flex;
    flex-direction: column-reverse;
    img{
      width: 36px;
      max-width: 20%;
      display: inline-block;
    }
  }
  .hud-inner--logs--player{
    color: $player-color--text;
  }
  .hud-inner--logs--monster{
    color: $monster-color--text;
  }

  .hud-inner--icons {
    grid-column: 1 / 7;
    grid-row: 1;
    text-align: center;
    font-weight: bold;
  }

  .hud-inner--icons--attack {
    grid-column: 2;
    grid-row: 1;
    color: $icon-color--attack;
  }

  .hud-inner--icons--heal {
    grid-column: 3;
    grid-row: 1;
    color: $icon-color--remedy;
  }

  .hud-inner--icons--special {
    grid-column: 4;
    grid-row: 1;
    color: $icon-color--special;
  }

  .hud-inner--icons--run {
    grid-column: 5;
    grid-row: 1;
    color: $icon-color--escape;
  }
</style>