<template>
  <div
    class="sw-card"
    :class="cardClasses"
  >
    <slot name="title">
      <div
        v-if="title"
        class="sw-card__title"
      >
        {{ title }}
      </div>
    </slot>

    <slot name="subtitle">
      <div
        v-if="subtitle"
        class="sw-card__subtitle"
      >
        {{ subtitle }}
      </div>
    </slot>

    <slot name="tabs" />

    <div
      v-if="!!$slots.toolbar"
      class="sw-card__toolbar"
    >
      <slot name="toolbar" />
    </div>

    <div class="sw-card__content">
      <div
        v-if="!!$slots['context-actions'] || !!$scopedSlots['context-actions']"
        class="sw-card__context-menu"
      >
        <sw-context-button>
          <slot name="context-actions" />
        </sw-context-button>
      </div>
      <slot />
      <slot
        name="grid"
        :title="title"
      />
      <slot name="footer" />

      <sw-loader v-if="isLoading" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: false,
      default: '',
    },
    subtitle: {
      type: String,
      required: false,
      default: '',
    },
    hero: {
      type: Boolean,
      required: false,
      default: false,
    },
    isLoading: {
      type: Boolean,
      required: false,
      default: false,
    },
    large: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  computed: {
    cardClasses() {
      return {
        'sw-card--tabs': !!this.$slots.tabs || !!this.$scopedSlots.tabs,
        'sw-card--grid': !!this.$slots.grid || !!this.$scopedSlots.grid,
        'sw-card--hero': !!this.hero,
        'sw-card--large': this.large,
        'has--header': !!this.$slots.toolbar || !!this.$scopedSlots.toolbar,
      };
    },
  },
};
</script>

<style lang="scss">
@import "../../assets/scss/variables";

.sw-card {
  max-width: 960px;
  margin: 0 auto 40px auto;
  position: relative;
  color: $color-darkgray-200;

  &__context-menu {
    position: absolute;
    top: 30px;
    right: 30px;
  }

  &.sw-card--grid {
    .sw-card__content {
      display: grid;
      padding: 0;

      .sw-grid {
        border-top: none;
      }
    }
  }

  &.sw-card--hero {
    .sw-card__content {
      background: none;
      border: none;
      text-align: center;

      h3 {
        font-size: 30px;
      }
    }
  }

  &.sw-card--large {
    max-width: 1330px;

    .sw-card__title,
    .sw-card__subtitle {
      width: auto;
      position: relative;
      top: 0;
      left: 0;
      text-align: left;
    }
  }

  .sw-card__title,
  .sw-card__subtitle {
    margin-bottom: 12px;
    color: $color-darkgray-200;

    @media screen and (min-width: 1580px) {
      width: 200px;
      position: absolute;
      left: -240px;
      text-align: right;
    }
  }

  .sw-card__title {
    font-size: $font-size-large;
    line-height: 24px;

    @media screen and (min-width: 1580px) {
      top: 0;
    }
  }

  .sw-card__subtitle {
    font-size: $font-size-small;
    line-height: 20px;

    @media screen and (min-width: 1580px) {
      top: 40px;
    }
  }

  .sw-card__toolbar {
    padding: 30px;
    background-color: $color-gray-100;
    border: 1px solid $color-gray-300;
  }

  &.has--header {
    .sw-card__content {
      border-top: none;
      border-radius: 0 0 $border-radius-default $border-radius-default;
    }
  }

  .sw-card__content {
    padding: 30px;
    background: $color-white;
    background-clip: padding-box;
    border-radius: $border-radius-default;
    border: 1px solid $color-gray-300;
    position: relative;

    @media screen and (max-width: 800px) {
      padding: 15px;
    }

    h1,
    h2,
    h3,
    h4,
    h5,
    h6 {
      font-weight: normal;
    }

    h1 {
      font-size: 24px;
    }

    h2 {
      font-size: 22px;
    }

    h3 {
      font-size: 20px;
    }

    h4,
    h5,
    h6 {
      font-size: 18px;
    }

    a.sw-card__quick-link {
      display: grid;
      grid-auto-flow: column;
      grid-column-gap: 6px;
      align-items: center;
      text-decoration: none;
      color: $color-shopware-brand-500;
      font-size: 14px;

      &:hover {
        color: $color-shopware-brand-600;
      }
    }

    .sw-tabs {
      border-width: 0 0 1px 0;
      border-bottom-right-radius: 0;
      border-bottom-left-radius: 0;
      margin-bottom: 0;

      .sw-tabs__container {
        position: relative;
        top: -5px;
      }
    }

    .sw-tabs-item {
      margin: 0;
      padding-top: 5px;
      line-height: 44px;
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;

      &:hover {
        border-bottom-color: $color-gray-300;
      }

      &:focus {
        background-color: $color-gray-100;
        border-bottom-color: $color-gray-300;
      }

      &:active {
        border-bottom-color: $color-white;
        background-color: $color-white;
      }

      &.router-link-active {
        background-color: $color-white;
        border-bottom-color: $color-white;
      }
    }
  }
}

.sw-card.sw-card--tabs {
  .sw-card__content {
    border-radius: 0 0 $border-radius-default $border-radius-default;
    border-top: none;
  }

  & > .sw-tabs {
    border: 1px solid $color-gray-300;
    border-radius: $border-radius-default $border-radius-default 0 0;
    border-bottom: none;
    margin-bottom: 0;

    .sw-tabs__content {
      background-color: $color-gray-50;
    }

    .sw-tabs__custom-Content {
      padding: 0;
    }

    .sw-tabs__arrow {
      top: (56px / 2);
      padding: 0 12px 0 12px;
    }

    &.sw-tabs--scrollable {
      padding: 0 (20px + 12px) 0 (20px + 12px);
    }

    .sw-tabs-item {
      padding-top: 19px;
      padding-bottom: 15px;
      font-weight: normal;

      &.sw-tabs-item--active {
        font-weight: 600;
      }
    }
  }
}
</style>
