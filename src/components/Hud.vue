<template>
    <div id="hud-inner">
      <div class="hud-inner--icons">
        <div class="hud-inner--icons--attack hud-icon">
          Attack
          <img src="@/assets/icons/hud/attack.svg"/>
        </div>
        <div class="hud-inner--icons--heal hud-icon">
          Heal
          <img src="@/assets/icons/hud/remedy.svg"/>
        </div>
        <div class="hud-inner--icons--special hud-icon" :class="{'hud-icon--disabled' : rage != 100}">
          Special attack
          <img src="@/assets/icons/hud/ember.svg"/>
        </div>
        <div class="hud-inner--icons--run hud-icon">
          Escape
          <img src="@/assets/icons/hud/run.svg"/>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Hud',
  data:()=>{
    return{
      rage: 0
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "@/assets/scss/_theme.scss";
  .hud-inner--icons {
    display: grid;
    grid-template-columns: 2fr repeat(4, 1fr) 2fr;
    grid-gap: 10px;
    grid-auto-rows: minmax(100px, auto);
    font-size: 1.5rem;
    text-align: center;
  }
  .hud-icon{
    img{
      margin: 0.25rem auto;
      display: block;
      width: 60%;
    }
  }
  .hud-icon--disabled{
    opacity: 0.5;
  }
  
  .hud-inner--icons {
    grid-column: 1 / 7;
    grid-row: 1;
    text-align: center;
    font-weight: bold;
  }

  .hud-inner--icons--attack{
    grid-column: 2;
    grid-row: 1;
    color: $icon-color--attack;
  }

  .hud-inner--icons--heal{
    grid-column: 3;
    grid-row: 1;
    color: $icon-color--remedy;
  }

  .hud-inner--icons--special{
    grid-column: 4;
    grid-row: 1;
    color: $icon-color--special;
  }

  .hud-inner--icons--run{
    grid-column: 5;
    grid-row: 1;
    color: $icon-color--escape;
  }
</style>
