<template>
  <span
    class="sw-button-group"
    :class="buttonGroupClasses"
  >
    <slot />
  </span>
</template>

<script>
export default {
  name: 'SwButtonGroup',

  props: {
    block: {
      type: Boolean,
      required: false,
      default: false,
    },

    splitButton: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  computed: {
    buttonGroupClasses() {
      return {
        'sw-button-group--block': this.block,
        'sw-button-group--split': this.splitButton,
      };
    },
  },
};
</script>

<style lang="scss">
@import '../../assets/scss/variables.scss';

.sw-button-group {
  display: inline-flex;
  vertical-align: middle;
  margin-right: 1px;
  position: relative;

  &.sw-button-group--block {
    display: flex;
  }

  .sw-tooltip--wrapper > button.sw-button {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }

  .sw-button {
    border-radius: 0;
    margin-left: -1px;
    position: relative;

    &:first-child {
      border-top-left-radius: $border-radius-default;
      border-bottom-left-radius: $border-radius-default;
      margin-left: 0;
    }

    &:last-child {
      border-top-right-radius: $border-radius-default;
      border-bottom-right-radius: $border-radius-default;
    }

    &:only-child {
      margin-left: -1px;
    }

    &.sw-button--primary,
    &.sw-button--danger,
    &.sw-button--contrast {
      position: static;
      margin-left: 0;
    }
  }

  .sw-context-button {
    .sw-button {
      border-radius: 0;
    }

    &:first-child .sw-button {
      border-top-left-radius: $border-radius-default;
      border-bottom-left-radius: $border-radius-default;
    }

    &:last-child .sw-button {
      border-top-right-radius: $border-radius-default;
      border-bottom-right-radius: $border-radius-default;
    }
  }

  &.sw-button-group--split {
    > .sw-button {
      &.sw-button--primary,
      &.sw-button--danger,
      &.sw-button--contrast {
        position: relative;
        margin-left: 8px;
      }
    }

    .sw-context-button > .sw-button {
      &.sw-button--primary,
      &.sw-button--danger,
      &.sw-button--contrast {
        border-left: 1px solid rgba(0, 0, 0, 0.2);
      }
    }
  }
}
</style>
