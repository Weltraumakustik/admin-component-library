<template>
  <span
    class="sw-color-badge"
    :class="variantClass"
    v-bind="$attrs"
    :style="colorStyle"
  >
    <slot />
  </span>
</template>

<script>
export default {
  name: 'SwColorBadge',
  props: {
    variant: {
      type: String,
      required: false,
      default: 'default',
    },
    color: {
      type: String,
      required: false,
      default: '',
    },
    rounded: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  computed: {
    colorStyle() {
      if (!this.color.length) {
        return '';
      }
      return `background:${this.color}`;
    },
    variantClass() {
      return {
        [`is--${this.variant}`]: true,
        'is--rounded': this.rounded,
      };
    },
  },
};
</script>

<style lang="scss">
@import "../../assets/scss/variables.scss";

$sw-color-badge-color-fallback: $color-gray-300;
$sw-color-badge-color-warning: $color-pumpkin-spice-500;
$sw-color-badge-color-error: $color-crimson-500;
$sw-color-badge-color-success: $color-emerald-500;
$sw-color-badge-color-info: $color-shopware-brand-500;

.sw-color-badge {
  display: inline-block;
  height: 8px;
  width: 8px;
  margin: 0 0 1px 10px;
  border-radius: 2px;
  background-color: $sw-color-badge-color-fallback;

  &.is--rounded {
    border-radius: 100%;
  }

  &.is--warning {
    background-color: $sw-color-badge-color-warning;
  }

  &.is--error,
  &.is--danger {
    background-color: $sw-color-badge-color-error;
  }

  &.is--success {
    background-color: $sw-color-badge-color-success;
  }

  &.is--info {
    background-color: $sw-color-badge-color-info;
  }
}

</style>
