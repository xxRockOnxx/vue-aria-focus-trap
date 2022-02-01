<template>
  <component
    :is="tag"
  >
    <div tabindex="0" />

    <div ref="content">
      <slot />
    </div>

    <div tabindex="0" />
  </component>
</template>

<script>
export default {
  props: {
    tag: {
      type: String,
      default: () => 'div',
    },
  },

  data() {
    return {
      ignoreUtilFocusChanges: false,
      lastFocused: null,
    };
  },

  mounted() {
    document.addEventListener('focus', this.trapFocus, true);
  },

  beforeDestroy() {
    document.removeEventListener('focus', this.trapFocus, true);
  },

  methods: {
    isFocusable(element) {
      if (element.tabIndex < 0) {
        return false;
      }

      if (element.disabled) {
        return false;
      }

      switch (element.nodeName) {
        case 'A':
          return !!element.href && element.rel !== 'ignore';

        case 'INPUT':
          return element.type !== 'hidden';

        case 'BUTTON':
        case 'SELECT':
        case 'TEXTAREA':
          return true;

        default:
          return false;
      }
    },

    attemptFocus(element) {
      if (!this.isFocusable(element)) {
        return false;
      }

      // Prevent running the focus callback
      // while we are trying to change to the right one.
      this.ignoreUtilFocusChanges = true;

      try {
        element.focus();
      } catch (e) {
        // continue regardless of error
      }

      this.ignoreUtilFocusChanges = false;

      return document.activeElement === element;
    },

    focusFirstDescendant(element) {
      for (let i = 0; i < element.childNodes.length; i++) {
        const child = element.childNodes[i];

        if (this.attemptFocus(child) || this.focusFirstDescendant(child)) {
          return true;
        }
      }

      return false;
    },

    focusLastDescendant(element) {
      for (let i = element.childNodes.length - 1; i >= 0; i--) {
        const child = element.childNodes[i];

        if (this.attemptFocus(child) || this.focusLastDescendant(child)) {
          return true;
        }
      }

      return false;
    },

    trapFocus(evt) {
      if (this.ignoreUtilFocusChanges) {
        return;
      }

      if (this.$refs.content.contains(evt.target)) {
        this.lastFocused = evt.target;
      } else {
        this.focusFirstDescendant(this.$refs.content);

        if (this.lastFocused === document.activeElement) {
          this.focusLastDescendant(this.$refs.content);
        }

        this.lastFocused = document.activeElement;
      }
    },
  },
};
</script>
