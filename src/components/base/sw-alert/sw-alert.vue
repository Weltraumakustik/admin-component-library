<template>
  <div
    class="sw-alert"
    :class="alertClasses"
    role="alert"
  >
    <slot name="customIcon">
      <sw-icon
        v-if="showIcon"
        size="20px"
        class="sw-alert__icon"
        :name="alertIcon"
        decorative
      />
    </slot>

    <div
      class="sw-alert__body"
      :class="alertBodyClasses"
    >
      <div
        v-if="title"
        class="sw-alert__title"
      >
        {{ title }}
      </div>

      <div class="sw-alert__message">
        <slot />
      </div>
    </div>

    <div
      v-if="hasActionSlot"
      class="sw-alert__actions"
    >
      <slot name="actions" />
    </div>

    <button
      v-if="closable"
      class="sw-alert__close"
      aria-label="Schließen"
      title="Schließen"
      @click.prevent="$emit('close', notificationIndex)"
    >
      <sw-icon
        name="small-default-x-line-medium"
        small
      />
    </button>
  </div>
</template>

<script>
import SwIcon from '../sw-icon/sw-icon.vue';

export default {
  name: 'SwAlert',

  components: {
    'sw-icon': SwIcon,
  },

  props: {
    variant: {
      type: String,
      default: 'info',
      validValues: ['info', 'warning', 'error', 'success'],
      validator(value) {
        return ['info', 'warning', 'error', 'success'].includes(value);
      },
    },
    appearance: {
      type: String,
      default: 'default',
      validValues: ['default', 'notification', 'system'],
      validator(value) {
        return ['default', 'notification', 'system'].includes(value);
      },
    },
    title: {
      type: String,
      required: false,
      default: '',
    },
    showIcon: {
      type: Boolean,
      required: false,
      default: true,
    },
    closable: {
      type: Boolean,
      required: false,
      default: false,
    },
    notificationIndex: {
      type: String,
      required: false,
      default: null,
    },
  },

  computed: {
    alertIcon() {
      const iconConfig = {
        info: 'default-badge-info',
        warning: 'default-badge-warning',
        error: 'default-badge-error',
        success: 'default-basic-checkmark-circle',
      };

      return iconConfig[this.variant] || 'default-bell-bell';
    },

    hasActionSlot() {
      return !!this.$slots.actions;
    },

    alertClasses() {
      return [
        `sw-alert--${this.variant}`,
        `sw-alert--${this.appearance}`,
        {
          'sw-alert--icon': this.showIcon,
          'sw-alert--no-icon': !this.showIcon,
          'sw-alert--closable': this.closable,
          'sw-alert--actions': this.hasActionSlot,
        },
      ];
    },

    alertBodyClasses() {
      return {
        'sw-alert__body--icon': this.showIcon,
        'sw-alert__body--closable': this.closable,
      };
    },
  },
};

</script>

<style lang="scss">
@import "../../assets/scss/variables.scss";

$sw-alert-box-shadow: 1px 2px 5px rgba(0, 0, 0, 0.1);
$sw-alert-size-close: 40px;

.sw-alert {
  border: 1px solid $color-gray-300;
  border-radius: $border-radius-default;
  color: $color-darkgray-200;
  background-color: $color-white;
  text-align: left;
  position: relative;
  margin: 0 auto 20px;
  font-size: $font-size-default;

  &__body {
    padding: 24px 60px 24px 24px;
    line-height: 1.5625;

    &--icon {
      padding: 24px 60px;
    }

    &--closable {
      padding-right: $sw-alert-size-close;
    }
  }

  &__icon {
    color: $color-gray-300;
    position: absolute;
    display: block;
    left: 26px;
    top: 28px;
  }

  &__close {
    width: $sw-alert-size-close;
    height: $sw-alert-size-close;
    line-height: $sw-alert-size-close;
    position: absolute;
    display: block;
    top: 12px;
    right: 6px;
    padding: 0;
    margin: 0;
    text-align: center;
    background: none;
    border: 0 none;
    outline: none;
    cursor: pointer;
  }

  &__title {
    margin-bottom: 3px;
    display: block;
    font-weight: bold;
  }

  &__actions {
    border-top: 1px solid $color-gray-300;
    display: flex;
    flex-wrap: wrap;

    .sw-button {
      border: 0;
      height: 40px;
      background: none;
      border-radius: 0;
      flex-basis: 0;
      flex-grow: 1;
      max-width: 100%;
      font-weight: 500;

      &:not(:last-child) {
        border-right: 1px solid $color-gray-300;
      }

      &:hover {
        background-color: $color-white;
      }
    }
  }

  &--notification {
    box-shadow: $sw-alert-box-shadow;
    border: 1px solid $color-gray-300;
    border-left: 4px solid $color-gray-300;
    background-color: $color-gray-100;

    .sw-alert__actions {
      border-color: $color-gray-300;

      .sw-button {
        color: $color-shopware-brand-500;
        border-color: $color-gray-300;

        &:hover {
          background-color: $color-gray-100;
        }
      }
    }
  }

  &--warning {
    border-color: $color-pumpkin-spice-500;
    background-color: $color-pumpkin-spice-50;
    color: $color-pumpkin-spice-900;

    &.sw-alert--notification {
      background-color: $color-white;
      border-color: $color-gray-300;
      border-left-color: $color-pumpkin-spice-500;
      color: $color-darkgray-200;
    }

    .sw-alert__icon,
    .sw-alert__close {
      color: $color-pumpkin-spice-900;
    }

    .sw-alert__actions {
      border-color: $color-pumpkin-spice-200;

      .sw-button {
        border-color: $color-pumpkin-spice-200;

        &:hover {
          background-color: $color-pumpkin-spice-100;
        }
      }
    }
  }

  &--error {
    border-color: $color-crimson-500;
    background-color: $color-crimson-50;

    &.sw-alert--notification {
      background-color: $color-white;
      border-color: $color-gray-300;
      border-left-color: $color-crimson-500;
    }

    .sw-alert__icon,
    .sw-alert__close {
      color: $color-crimson-500;
    }

    .sw-alert__actions {
      border-color: $color-crimson-200;

      .sw-button {
        border-color: $color-crimson-200;

        &:hover {
          background-color: $color-crimson-100;
        }
      }
    }
  }

  &--success {
    border-color: $color-emerald-500;
    background-color: $color-emerald-50;

    &.sw-alert--notification {
      background-color: $color-white;
      border-color: $color-gray-300;
      border-left-color: $color-emerald-500;
    }

    .sw-alert__icon,
    .sw-alert__close {
      color: $color-emerald-500;
    }

    .sw-alert__actions {
      border-color: $color-emerald-200;

      .sw-button {
        border-color: $color-emerald-200;

        &:hover {
          background-color: $color-emerald-100;
        }
      }
    }
  }

  &--info {
    border-color: $color-shopware-brand-500;
    background-color: $color-shopware-brand-50;

    &.sw-alert--notification {
      background-color: $color-white;
      border-color: $color-gray-300;
      border-left-color: $color-shopware-brand-500;
    }

    .sw-alert__icon,
    .sw-alert__close {
      color: $color-shopware-brand-500;
    }

    .sw-alert__actions {
      border-color: $color-shopware-brand-200;

      .sw-button {
        border-color: $color-shopware-brand-200;

        &:hover {
          background-color: $color-shopware-brand-100;
        }
      }
    }
  }

  &--system {
    color: $color-white;
    background: $color-menu-start;
    border: 1px solid $color-darkgray-200;
    box-shadow: none;

    .sw-alert__actions {
      border-color: $color-darkgray-50;

      .sw-button {
        color: $color-gray-300;
        border-color: $color-darkgray-50;

        &:hover {
          background-color: lighten($color-menu-start, 6%);
        }

        &:active {
          background-color: lighten($color-menu-start, 8%);
        }
      }
    }
  }

  ul {
    padding: 8px 0 8px 20px;
  }
}

</style>
