<template>
  <div class="sw-loader">
    <div
      class="sw-loader__container"
      :style="loaderSize"
    >
      <div class="sw-loader-element">
        <div :style="{ borderWidth: borderWidth }" />
        <div :style="{ borderWidth: borderWidth }" />
        <div :style="{ borderWidth: borderWidth }" />
        <div :style="{ borderWidth: borderWidth }" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SwLoader',

  props: {
    size: {
      type: String,
      required: false,
      default: '50px',
      validator(value) {
        return /^(12|[2-9][0-9]|[1-9][2-9]|[1-9]\d{2,})px$/.test(value);
      },
    },
  },

  computed: {
    loaderSize() {
      return {
        width: `${this.numericSize}px`,
        height: `${this.numericSize}px`,
      };
    },
    numericSize() {
      const numericSize = parseInt(this.size, 10);

      if (Number.isNaN(numericSize)) {
        return 50;
      }

      if (numericSize < 12) {
        return 50;
      }

      return numericSize;
    },

    borderWidth() {
      return `${Math.floor(this.numericSize / 12)}px`;
    },
  },
};
</script>

<style lang="scss">
@import "../../assets/scss/variables.scss";

$sw-loader-color-overlay: rgba(255, 255, 255, 0.8);
$sw-loader-element-color: $color-shopware-brand-500;
$sw-loader-rotate-duration: 1.4s;
$sw-loader-z-index: $z-index-loader;

.sw-loader {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  z-index: $sw-loader-z-index;
  background: $sw-loader-color-overlay;

  .sw-loader__container {
    display: grid;
    grid-auto-columns: auto;
    text-align: center;
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .sw-loader-element {
    div {
      position: absolute;
      width: 100%;
      height: 100%;
      border-width: 4px;
      border-style: solid;
      border-radius: 50%;
      border-color: $sw-loader-element-color transparent transparent transparent;
      animation: sw-loader-rotator $sw-loader-rotate-duration cubic-bezier(0.5, 0, 0.5, 1) infinite;

      &:nth-child(1) {
        animation-delay: -0.45s;
      }

      &:nth-child(2) {
        animation-delay: -0.3s;
      }

      &:nth-child(3) {
        animation-delay: -0.15s;
      }
    }
  }

  @keyframes sw-loader-rotator {
    0% {
      transform: rotate(0deg);
    }

    100% {
      transform: rotate(360deg);
    }
  }
}
</style>
