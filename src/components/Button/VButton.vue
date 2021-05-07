<template>
  <button
      v-bind="$attrs"
      v-on="$listeners"
      :class="['button', {'button--icon': hasIcon, 'button--uppercase': uppercase}]"
  >
    <slot />

    <slot name="icon" :icon="icon">
      <v-icon v-if="hasIcon" :glyph="icon" />
    </slot>
  </button>
</template>

<script>
import { VIcon } from "@/components/Icon";

export default {
  name: 'HelloWorld',

  inheritAttrs: false,

  components: {
    VIcon
  },

  props: {
    icon: {
      type: [String, null],
      default: null,
    },

    uppercase: {
      type: Boolean,
      default: false,
    }
  },

  computed: {
    hasIcon() {
      return !!this.icon;
    },
  },
}
</script>

<style lang="scss" scoped>
button {
  display: inline-block;
  border-style: solid;
  border-width: 1px;
  text-align: left;
  font-family: inherit;
  transition: border-color $duration, color $duration;
  border-color: $border-color;
  background: transparent;
  color: inherit;
  vertical-align: middle;
  white-space: nowrap;
  user-select: none;
  cursor: pointer;
  padding: 0.1em 0.3em;
  margin: 0;
  outline: none;

  &--uppercase { text-transform: uppercase; }

  &::v-deep .icon {
    fill: $border-color
  }

  &:hover {
    border-color: $primary-color;
    color: $primary-color;

    &::v-deep .icon {
      fill: $primary-color
    }
  }

  &--icon::v-deep .icon {
    margin-left: 1em;
  }
}
</style>
