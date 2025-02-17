<template>
  <ul class="sw-select-selection-list">
    <!-- eslint-disable vue/no-use-v-if-with-v-for -->
    <li
      v-for="(selection, index) in selections"
      v-if="!hideLabels"
      :key="selection[valueProperty]"
      class="sw-select-selection-list__item-holder"
      :class="'sw-select-selection-list__item-holder--' + index"
      :data-id="selection[valueProperty]"
    >
      <slot
        name="selected-option"
        v-bind="{ selection, defaultLabel: selection[labelProperty], disabled }"
      >
        <sw-label
          :dismissable="!isSelectionDisabled(selection)"
          :size="size"
          @dismiss="onClickDismiss(selection)"
        >
          <span class="sw-select-selection-list__item">
            <slot
              name="label-property"
              v-bind="{ item: selection, index, labelProperty, valueProperty }"
            >
              {{ selection[labelProperty] }}
            </slot>
          </span>
        </sw-label>
      </slot>
    </li>

    <li
      v-if="invisibleCount > 0 && !hideLabels"
      class="sw-select-selection-list__load-more"
    >
      <slot
        name="invisible-count"
        v-bind="{ invisibleCount, onClickInvisibleCount }"
      >
        <sw-button
          class="sw-select-selection-list__load-more-button"
          @click.stop="onClickInvisibleCount"
        >
          +{{ invisibleCount }}
        </sw-button>
      </slot>
    </li>

    <li>
      <slot
        name="input"
        v-bind="{ placeholder, searchTerm, onSearchTermChange, onKeyDownDelete }"
      >
        <!-- eslint-disable-next-line vuejs-accessibility/form-control-has-label -->
        <input
          ref="swSelectInput"
          class="sw-select-selection-list__input"
          type="text"
          :disabled="disabled"
          :readonly="!enableSearch"
          :placeholder="showPlaceholder"
          :value="searchTerm"
          @input="onSearchTermChange"
          @keydown.delete="onKeyDownDelete"
        >
      </slot>
    </li>
  </ul>
</template>

<script>
import SwLabel from '../../../base/sw-label/sw-label.vue';
import SwButton from '../../../base/sw-button/sw-button.vue';

export default {
  name: 'SwSelectSelectionList',

  // inject: ['feature'],

  components: {
    'sw-label': SwLabel,
    'sw-button': SwButton,
  },

  props: {
    selections: {
      type: Array,
      required: false,
      default: () => [],
    },
    labelProperty: {
      type: String,
      required: false,
      default: 'label',
    },
    valueProperty: {
      type: String,
      required: false,
      default: 'value',
    },
    enableSearch: {
      type: Boolean,
      required: false,
      default: true,
    },
    invisibleCount: {
      type: Number,
      required: false,
      default: 0,
    },
    size: {
      type: String,
      required: false,
      default: null,
    },
    alwaysShowPlaceholder: {
      type: Boolean,
      required: false,
      default: false,
    },
    placeholder: {
      type: String,
      required: false,
      default: '',
    },
    isLoading: {
      type: Boolean,
      required: false,
      default: false,
    },
    searchTerm: {
      type: String,
      required: false,
      default: '',
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false,
    },
    selectionDisablingMethod: {
      type: Function,
      required: false,
      default: () => false,
    },
    hideLabels: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  computed: {
    showPlaceholder() {
      // if (this.feature.isActive('FEATURE_NEXT_7530')) {
      //   return (this.alwaysShowPlaceholder || (this.selections.length === 0 && this.hideLabels))
      //     ? this.placeholder
      //     : '';
      // }

      return (this.selections.length > 0 && !this.hideLabels) ? '' : this.placeholder;
    },
  },

  methods: {
    isSelectionDisabled(selection) {
      if (this.disabled) {
        return true;
      }

      return this.selectionDisablingMethod(selection);
    },

    onClickInvisibleCount() {
      this.$emit('total-count-click');
    },

    onSearchTermChange(event) {
      this.$emit('search-term-change', event.target.value, event);
    },

    onKeyDownDelete() {
      if (this.searchTerm.length < 1) {
        this.$emit('last-item-delete');
      }
    },

    onClickDismiss(item) {
      this.$emit('item-remove', item);
    },

    focus() {
      this.$refs.swSelectInput.focus();
    },

    blur() {
      this.$refs.swSelectInput.blur();
    },

    select() {
      this.$refs.swSelectInput.select();
    },

    getFocusEl() {
      return this.$refs.swSelectInput;
    },
  },
};
</script>

<style lang="scss">
@import "../../../assets/scss/variables.scss";

.sw-select-selection-list {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  width: calc(100% - 30px);

  .sw-select-selection-list__item-holder {
    max-width: 220px;
    line-height: 0;
    margin: 8px 6px 0 0;
  }

  button.sw-select-selection-list__load-more-button {
    padding: 8px 12px;
    margin: 0 6px 0 0;
    color: $color-shopware-brand-500;
    font-size: 12px;
    line-height: 14px;
    border-radius: 2px;
    height: unset;
    border-color: $color-gray-300;
  }

  .sw-select-selection-list__input {
    display: inline-block;
    min-width: 200px;
    padding: 12px 16px 12px 8px;

    &::placeholder {
      color: lighten($color-darkgray-200, 25%);
    }
  }
}

.sw-field--medium .sw-select-selection-list {
  .sw-select-selection-list__item-holder .sw-label {
    margin: 4px 6px 0 0;
  }

  input {
    padding: 4px 16px 8px 8px;
  }

  .sw-select-selection-list__load-more-button {
    padding: 4px 12px;
    margin: 4px 6px 0 0;
  }
}

.sw-field--small .sw-select-selection-list {
  .sw-select-selection-list__item-holder .sw-label {
    margin: 4px 6px 0 0;
  }

  input {
    padding: 2px 16px 4px 8px;
  }

  .sw-select-selection-list__load-more-button {
    padding: 0 8px;
    margin: -1px 6px 0 0;
  }
}
</style>
