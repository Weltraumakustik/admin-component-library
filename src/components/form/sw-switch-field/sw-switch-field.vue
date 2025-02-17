<template>
  <div
    class="sw-field--switch"
    :class="swSwitchFieldClasses"
  >
    <div class="sw-field--switch__content">
      <div class="sw-field--switch__input">
        <!-- eslint-disable-next-line vuejs-accessibility/form-control-has-label -->
        <input
          :id="identification"
          type="checkbox"
          :name="formFieldName || identification"
          :checked="inputState"
          :disabled="disabled"
          @change="onChange"
        >
        <div class="sw-field__switch-state">
          <div class="sw-field__switch-state-knob" />
        </div>
      </div>

      <sw-base-field
        v-bind="$attrs"
        :disabled="disabled"
        :name="identification"
        :is-inheritance-field="isInheritanceField"
        :is-inherited="isInherited"
        @inheritance-restore="onInheritanceRestore($event)"
        @inheritance-remove="$emit('inheritance-remove', $event)"
      >
        <template #label>
          <slot name="label" />
        </template>
      </sw-base-field>
    </div>
    <sw-field-error :error="error" />
  </div>
</template>

<script>
import SwCheckboxField from '../sw-checkbox-field/sw-checkbox-field.vue';

export default {
  name: 'SwSwitchField',

  extends: SwCheckboxField,

  inheritAttrs: false,

  props: {
    noMarginTop: {
      type: Boolean,
      required: false,
      default: false,
    },

    size: {
      type: String,
      required: false,
      default: 'default',
      validValues: ['small', 'medium', 'default'],
      validator(val) {
        return ['small', 'medium', 'default'].includes(val);
      },
    },
  },

  computed: {
    swSwitchFieldClasses() {
      return [
        {
          'sw-field--switch-bordered': this.bordered,
          'sw-field--switch-no-margin-top': this.noMarginTop,
          ...this.swCheckboxFieldClasses,
        },
        `sw-field--${this.size}`,
      ];
    },
  },

  methods: {
    onInheritanceRestore(event) {
      this.$emit('inheritance-restore', event);
    },
  },
};
</script>

<style lang="scss">
@import "../../assets/scss/variables.scss";

$sw-field--switch-color-border: $color-gray-300;
$sw-field--switch-color-background: $color-white;
$sw-field--switch-color-text: $color-darkgray-200;
$sw-field--switch-color-focus: $color-shopware-brand-500;
$sw-field--switch-color-error: $color-crimson-500;

.sw-field--switch {
  margin-top: 24px;
  margin-bottom: 22px;

  &.sw-field--switch-no-margin-top {
    margin-top: 0;
  }

  .sw-field {
    margin-bottom: 0;
  }

  .sw-field__label {
    margin-bottom: 0;

    label {
      cursor: pointer;
      flex-grow: initial;
      margin-right: auto;
      padding: 15px 10px;
    }
  }

  .sw-field--switch__content {
    display: grid;
    grid-template-columns: 24px 1fr auto;
    align-items: center;
    color: $sw-field--switch-color-text;
  }

  &.sw-field--switch-bordered .sw-field--switch__content {
    border-radius: 4px;
    border: 1px solid $sw-field--switch-color-border;
    padding: 0 16px;
  }

  .sw-field--switch__input {
    position: relative;
    padding: 15px 0;
    width: 24px;
    height: 100%;

    input[type="checkbox"] {
      opacity: 0;
      display: block;
      width: 100%;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
      border: 0 none;
      background: none;
      -webkit-appearance: none;
      z-index: 2;
      cursor: pointer;

      &:disabled {
        cursor: auto;
      }

      &:disabled ~ .sw-field__switch-state {
        background: $color-gray-100;

        .sw-field__switch-state-knob {
          background: $sw-field--switch-color-border;
        }
      }

      &:checked ~ .sw-field__switch-state {
        background: $sw-field--switch-color-focus;

        .sw-field__switch-state-knob {
          left: 10px;
        }
      }

      &:checked:disabled ~ .sw-field__switch-state {
        background: $color-gray-100;
      }
    }

    .sw-field__switch-state {
      position: absolute;
      width: 100%;
      height: 16px;
      z-index: 1;
      text-align: center;
      background: $sw-field--switch-color-border;
      border-radius: 8px;

      .sw-field__switch-state-knob {
        transition: all 0.3s ease-in-out;
        width: 10px;
        height: 10px;
        position: absolute;
        top: 3px;
        left: 3px;
        background: $sw-field--switch-color-background;
        border-radius: 7px;
      }
    }
  }

  .sw-field__inheritance-icon {
    display: flex;
    align-items: center;
    margin-left: 8px;
    margin-right: 0;
  }

  &.sw-field--small.sw-field--switch-bordered {
    label {
      padding: 8px 10px;
    }

    .sw-field--switch__input {
      padding: 8px 0;
    }
  }

  &.sw-field--small {
    label {
      padding: 8px 10px;
    }

    .sw-field--switch__input {
      padding: 8px 0;
    }
  }

  &.sw-field--medium.sw-field--switch-bordered {
    label {
      padding: 11px 10px;
    }

    .sw-field--switch__input {
      padding: 11px 0;
    }
  }

  &.has--error {
    .sw-field__switch-state {
      background: $sw-field--switch-color-error;
    }

    input[type="checkbox"] {
      &:disabled ~ .sw-field__switch-state {
        background: $sw-field--switch-color-error;

        .sw-field__switch-state-knob {
          background: darken($sw-field--switch-color-error, 5%);
        }
      }

      &:checked ~ .sw-field__switch-state {
        background: $sw-field--switch-color-error;
      }

      &:checked:disabled ~ .sw-field__switch-state {
        background: $sw-field--switch-color-error;
      }
    }
  }

  &.is--inherited {
    input[type="checkbox"] {
      &:checked ~ .sw-field__switch-state {
        background: $sw-field--switch-color-border;
      }
    }
  }

  &:focus-within {
    .sw-field__switch-state {
      border-color: $color-shopware-brand-500;
      box-shadow: 0 0 4px $color-shopware-brand-500;
    }
  }
}
</style>
