<template>
  <sw-base-field
    class="sw-field--radio"
    :class="classes"
    :name="formFieldName"
    v-bind="$attrs"
  >
    <template #sw-field-input="{ identification, helpText, error, disabled }">
      <sw-help-text
        v-if="helpText"
        :text="helpText"
      />

      <div class="sw-field__radio-group">
        <div
          v-for="(option, index) in options"
          :key="index"
          class="sw-field__radio-option group-bordered"
          :class="{ 'sw-field__radio-option-checked': currentIndex === index }"
        >
          <div class="sw-field__radio-input">
            <input
              :id="`${identification}-${index}`"
              type="radio"
              :name="identification"
              :value="index"
              :disabled="disabled || option.disabled"
              :checked="currentIndex === index"
              @change="onChange"
            >
            <div class="sw-field__radio-state" />
          </div>

          <!-- eslint-disable-next-line vuejs-accessibility/label-has-for -->
          <label :for="`${identification}-${index}`">
            <span>{{ option.name }}</span>
            <sw-help-text
              v-if="option.helpText"
              :text="option.helpText"
            />

            <div
              v-if="option.description"
              class="sw-field__radio-option-description"
            >
              {{ option.description }}
            </div>
          </label>

          <slot
            :name="`custom-field-${option.value}`"
            v-bind="{ option, disabled, checked: currentIndex === index }"
          />
        </div>
      </div>

      <div
        v-if="description"
        class="sw-field__radio-description"
      >
        {{ description }}
      </div>
    </template>

    <template #label>
      <slot name="label" />
    </template>
  </sw-base-field>
</template>

<script>
import SwBaseField from '../_internal/sw-base-field/sw-base-field.vue';
import SwHelpText from '../../base/sw-help-text/sw-help-text.vue';
import SwFormFieldMixin from '../../../mixins/form-field.mixin';

export default {
  name: 'SwRadioField',

  components: {
    'sw-base-field': SwBaseField,
    'sw-help-text': SwHelpText,
  },

  mixins: [
    SwFormFieldMixin,
    // Mixin.getByName('remove-api-error'),
  ],

  inheritAttrs: false,

  model: {
    prop: 'value',
    event: 'change',
  },

  props: {
    bordered: {
      type: Boolean,
      required: false,
      default: false,
    },

    block: {
      type: Boolean,
      required: false,
      default: false,
    },

    description: {
      type: String,
      required: false,
      default: null,
    },

    options: {
      type: Array,
      required: false,
      default: () => [],
    },
    // FIXME: add type and default attribute to property
    // eslint-disable-next-line vue/require-prop-types, vue/require-default-prop
    value: {
      required: false,
    },
  },

  computed: {
    classes() {
      return [{
        'sw-field--radio-bordered': this.bordered,
        'sw-field--radio-block': this.block,
      }];
    },
    currentIndex() {
      const foundIndex = this.options.findIndex((item) => item.value === this.value);

      if (foundIndex < 0) {
        console.warn(`Given value "${this.value}" does not exists in given options`);
      }

      return foundIndex;
    },
  },

  methods: {
    onChange(event) {
      const selectedIndex = event.target.value;

      if (this.options[selectedIndex] === undefined) {
        console.warn(`Selected index "${this.value}" does not exists in given options`);
      }

      this.$emit('change', this.options[selectedIndex].value);
    },
  },
};
</script>

<style lang="scss">
@import '../../assets/scss/variables.scss';

$sw-field--radio-color-border: $color-gray-300;

.sw-field--radio {
  label ~ .sw-field__radio-group,
  .sw-help-text ~ .sw-field__radio-group {
    margin-top: 12px;
  }

  .sw-field__radio-option {
    display: grid;
    grid-template-columns: 16px 1fr 16px;
    grid-column-gap: 8px;
    margin-bottom: 12px;

    label {
      font-weight: normal;
      font-size: $font-size-small;
      color: $color-darkgray-200;

      .sw-help-text {
        margin-left: 5px;
      }
    }
  }

  &.sw-field--radio-bordered .sw-field__radio-group {
    border-radius: 4px;
    border: 1px solid $sw-field--radio-color-border;
    padding: 12px 16px;
    margin-bottom: 0;
  }

  .sw-field__radio-input {
    width: 16px;
    height: 16px;
    position: relative;

    input[type="radio"] {
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
      cursor: pointer;
      z-index: 2;

      &:disabled ~ .sw-field__radio-state {
        background: $color-gray-100;
        border-color: $color-gray-300;
        color: lighten($color-darkgray-200, 40%);
      }

      &:checked ~ .sw-field__radio-state {
        border: 5px solid $color-shopware-brand-500;
      }

      &:checked:disabled ~ .sw-field__radio-state {
        background: $color-gray-300;
        border: 1px solid $color-gray-300;
        box-shadow: inset 0 0 0 4px $color-gray-100;
        color: lighten($color-darkgray-200, 40%);
      }
    }

    .sw-field__radio-state {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 1;
      background: $color-white;
      color: $color-darkgray-200;
      border: 1px solid $color-gray-300;
      border-radius: 8px;
    }
  }

  &.has--error {
    .sw-field__radio-state {
      border-color: $color-crimson-500;
    }

    input[type="radio"]:checked ~ .sw-field__radio-state {
      border: 5px solid $color-crimson-500;
    }
  }

  .sw-field__radio-option-description {
    margin-top: 8px;
    color: $color-darkgray-200;
    font-size: $font-size-small;
    font-weight: normal;
    line-height: 22px;
  }

  .sw-field__radio-description {
    color: $color-gray-500;
    font-size: $font-size-small;
    line-height: 22px;
    padding-top: 8px;
  }

  &.sw-field--radio-block {
    .sw-field__label {
      font-size: $font-size-default;
      font-weight: bold;
      line-height: 22px;
      margin-bottom: 20px;
    }

    .sw-field__radio-option {
      border: 1px solid $color-gray-200;
      border-radius: $border-radius-default;
      padding: 12px 19px;
    }

    .sw-field__radio-option label {
      font-size: $font-size-default;
      font-weight: $font-weight-semi-bold;
      line-height: 22px;
      padding-left: 12px;
    }

    .sw-field__radio-input {
      margin-top: 5px;
    }

    .sw-field__radio-option.sw-field__radio-option-checked {
      background: rgba($color-shopware-brand-500, 0.05);
      border: 1px solid $color-shopware-brand-500;
    }
  }
}
</style>
