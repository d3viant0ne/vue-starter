<template>
  <button :class="cssClasses" @click="onClick">
    <slot v-if="!loading" />
    <vue-loader v-if="loading" />
  </button>
</template>

<script lang="ts">
  import VueLoader from '../VueLoader/VueLoader';

  export default {
    props: {
      primary: {
        type: Boolean,
        required: false,
        default: false,
      },
      accent: {
        type: Boolean,
        required: false,
        default: false,
      },
      warn: {
        type: Boolean,
        required: false,
        default: false,
      },
      disabled: {
        type: Boolean,
        required: false,
        default: false,
      },
      loading: {
        type: Boolean,
        required: false,
        default: false,
      },
      pulse: {
        type: Boolean,
        required: false,
        default: false,
      },
    },
    components: {
      VueLoader,
    },
    methods: {
      onClick(e: Event) {
        if (this.disabled === false && this.loading === false) {
          this.$emit('click', e);
        }
      },
    },
    computed: {
      cssClasses() {
        const classes = [this.$style.button, this.$style.ripple];

        if (this.primary) {
          classes.push(this.$style.primary);
        }

        if (this.accent) {
          classes.push(this.$style.accent);
        }

        if (this.warn) {
          classes.push(this.$style.warn);
        }

        if (this.disabled || this.loading) {
          classes.push(this.$style.disabled);
        }

        if (this.pulse) {
          classes.push(this.$style.pulse);
        }

        return classes;
      },
    },
  };
</script>

<style lang="scss" module>
  @import "../../variables";

  button,
  button:active,
  button:focus,
  button:hover {
    outline: none !important;
  }

  .button {
    color:                       $button-default-color;
    display:                     inline-block;
    margin:                      $button-margin;
    padding:                     $button-padding;
    text-align:                  center;
    vertical-align:              middle;
    touch-action:                manipulation;
    cursor:                      pointer;
    border:                      1px solid transparent;
    white-space:                 nowrap;
    text-transform:              uppercase;
    min-width:                   $button-min-width;
    position:                    relative;
    overflow:                    hidden;
    font-family:                 $button-font-family;
    font-size:                   $button-font-size;
    font-weight:                 $button-font-weight;
    border-radius:               $button-border-radius;
    background:                  $button-default-bg;
    box-shadow:                  $button-shadow;
    transition:                  $button-transition;
    transition-property:         box-shadow, background-color;
    height:                      $button-height;
    -webkit-tap-highlight-color: transparent;

    &:active {
      background: $button-default-hover-bg;
      box-shadow: $button-active-shadow;
    }

    &.primary {
      color:      $button-primary-color;
      background: $button-primary-bg;

      &:hover {
        background: $button-primary-hover-bg;
      }

      :global {
        .vue-loader-path {
          stroke: $button-primary-color;
        }
      }
    }

    &.accent {
      color:      $button-accent-color;
      background: $button-accent-bg;

      &:hover {
        background: $button-accent-hover-bg;
      }

      :global {
        .vue-loader-path {
          stroke: $button-accent-color;
        }
      }
    }

    &.warn {
      color:      $button-warn-color;
      background: $button-warn-bg;

      &:hover {
        background: $button-warn-hover-bg;
      }

      :global {
        .vue-loader-path {
          stroke: $button-warn-color;
        }
      }
    }

    &.disabled,
    &[disabled],
    fieldset[disabled] & {
      opacity:    .6;
      cursor:     not-allowed;
      box-shadow: none;

      &:hover {
        box-shadow: none;
      }
    }

    &.pulse {
      animation: loading-animation 1s infinite ease-in-out both;

      @keyframes loading-animation {
        0% {
          transform: scale(1);
        }
        25% {
          transform: scale(0.98);
        }
        50% {
          transform: scale(1);
        }
        75% {
          transform: scale(0.98);
        }
        100% {
          transform: scale(1);
        }
      }
    }

    :global {
      .vue-loader {
        position:    absolute;
        left:        50%;
        margin-left: -($grid-unit * 2);
        top:         $grid-unit;

        .vue-loader-circle {
          &:before {
            background: $button-default-color;
          }
        }
      }
    }
  }

  .ripple {
    position:  relative;
    overflow:  hidden;
    transform: translate3d(0, 0, 0);

    &:before {
      content:             "";
      display:             block;
      position:            absolute;
      width:               100%;
      height:              100%;
      top:                 0;
      left:                0;
      pointer-events:      none;
      background-image:    radial-gradient(circle, #000 10%, transparent 10.01%);
      background-repeat:   no-repeat;
      background-position: 50%;
      transform:           scale(10, 10);
      opacity:             0;
      transition:          transform .5s, opacity .5s;
    }

    &:active:before {
      transform:  scale(0, 0);
      opacity:    .2;
      transition: 0s;
    }
  }
</style>
