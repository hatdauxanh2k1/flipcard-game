<template>
  <div
    class="card"
    :class="{ 'is-disabled': isDisabled }"
    :style="{
      height: `${
        (windowHeight - 16 * 4) / Math.sqrt(cardContext.length) - 16
      }px`,
      width: `${
        (((windowHeight - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((windowHeight - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
          4) *
        2
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard()"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((windowHeight - 16 * 4) / Math.sqrt(cardContext.length) - 16) *
                3) /
              4 /
              3
            }px ${
              (((windowHeight - 16 * 4) / Math.sqrt(cardContext.length) - 16) *
                3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card__content">
          <img :src="require('@/assets/images/' + imgBackPath)" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackPath: {
      type: String,
      require: true,
    },
    card: {
      type: [String, Number, Object, Array],
    },
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    rules: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
      windowHeight: window.innerHeight,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.rules.length === 2) {
        return false;
      }
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped == false) {
        this.rules.length = 0;
      }
      if (this.isFlipped == true) {
        this.$emit("onFlip", this.card);
      }
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  width: 90px;
  height: 120px;
}

.card__inner {
  width: 100%;
  height: 100%;
  transform: rotate();
  transform-style: preserve-3d;
  transition: transform 1s;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card.is-disabled .card__inner {
  cursor: default;
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  box-shadow: 0 3px 10px 4px rgba(255, 255, 255, 0.2);
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--front .card__content {
  background: url(../assets/images/icon_back.png) no-repeat center center;
  width: 100%;
  height: 100%;
}
.card__face--back .card__content {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.card__face--back .card__content img {
  max-width: 100%;
  height: auto;
}
</style>
