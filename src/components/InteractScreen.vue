<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((windowHeight - 16 * 4) / Math.sqrt(cardContext.length) - 16) *
            3) /
            4 +
            16) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackPath="`${card}.png`"
        :card="{ index, value: card }"
        :cardContext="cardContext"
        @onFlip="checkRule($event)"
        :rules="rules"
      />
    </div>
  </div>
</template>

<script>
import CardScreen from "./CardScreen.vue";

export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip: CardScreen,
  },
  data() {
    return {
      rules: [],
      windowHeight: window.innerHeight,
    };
  },
  methods: {
    checkRule(card) {
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value &&
        this.rules[0].index !== this.rules[1].index
      ) {
        // console.log("Right..");
        //add class 'is-disable' to components card
        this.$refs[`card-${this.rules[0].index}`][0].onDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onDisableMode();
        //reset rules to []
        this.rules = [];

        //Check end game:
        const disabledElements = document.querySelectorAll(
          ".screen .card.is-disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        // console.log("Wrong....");
        setTimeout(() => {
          //close two card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

          //reset rules to []
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
