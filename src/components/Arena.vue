<template>
    <div>
      <div id="arena-inner">
        <Character id="arena-inner--player" char="player" :maxHp="120" :maxRage="100" :minDamage="3" :maxDamage="10" :minHeal="8" :maxHeal="14"/>
        <Character id="arena-inner--monster" char="monster"  :maxHp="150" :maxRage="200" :minDamage="6" :maxDamage="13" :minHeal="0" :maxHeal="0"/>
      </div>
    </div>
</template>

<script>
import Character from '@/components/Character.vue'
export default {
  name: 'Arena',
  components: {
    Character
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#arena-inner{
    display: grid;
    grid-template-columns: 2fr 1fr 2fr;
    grid-gap: 10px;
    grid-auto-rows: minmax(100px, auto);
}

  #arena-inner--player {
    grid-column: 1 / 2;
    grid-row: 1;
  }

  #arena-inner--monster {
    grid-column: 3 / 4;
    grid-row: 1;
  }
</style>
