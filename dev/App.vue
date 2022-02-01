<template>
  <div>
    <button
      ref="trigger"
      type="button"
      @click="openDialog"
    >
      Add Delivery Address
    </button>

    <p style="margin-top: 16px;">
      This component <b>only</b> handles the trapping of focus.<br/>
      Unlike the example from the w3.org website, it doesn not handle:

      <ul style="margin-top: 16px;">
        <li>What child element to focus on open</li>
        <li>Focusing of trigger element on close</li>
      </ul>
    </p>

    <p>
      <span>Source:</span>

      <a href="https://www.w3.org/TR/wai-aria-practices-1.2/examples/dialog-modal/dialog.html">
        https://www.w3.org/TR/wai-aria-practices-1.2/examples/dialog-modal/dialog.html
      </a>
    </p>

    <FocusTrap
      v-show="open"
      id="dialog1"
      role="dialog"
      aria-labelledby="dialog1_label"
      aria-modal="true"
    >
      <h2 id="dialog1_label" class="dialog_label">Add Delivery Address</h2>

      <div class="dialog_form">
        <div class="dialog_form_item">
          <label>
            <span class="label_text">Street:</span>
            <input ref="input" type="text" class="wide_input" />
          </label>
        </div>

        <div class="dialog_form_item">
          <label>
            <span class="label_text">City:</span>
            <input type="text" class="city_input" />
          </label>
        </div>

        <div class="dialog_form_item">
          <label>
            <span class="label_text">State:</span>
            <input type="text" class="state_input" />
          </label>
        </div>

        <div class="dialog_form_item">
          <label>
            <span class="label_text">Zip:</span>
            <input type="text" class="zip_input" />
          </label>
        </div>

        <div class="dialog_form_item">
          <label for="special_instructions">
            <span class="label_text">Special instructions:</span>
          </label>
          <input
            id="special_instructions"
            type="text"
            aria-describedby="special_instructions_desc"
            class="wide_input"
          />
          <div class="label_info" id="special_instructions_desc">
            For example, gate code or other information to help the driver find you
          </div>
        </div>
      </div>

      <div class="dialog_form_actions">
        <button
          type="button"
          @click="closeDialog"
        >
          Cancel
        </button>
      </div>
    </FocusTrap>
  </div>
</template>

<script>
import FocusTrap from '../src';

export default {
  components: {
    FocusTrap,
  },

  data() {
    return {
      open: false,
    }
  },

  methods: {
    openDialog() {
      this.open = true;

      this.$nextTick(() => {
        this.$refs.input.focus();
      });
    },

    closeDialog() {
      this.open = false;
      
      this.$nextTick(() => {
        this.$refs.trigger.focus();
      });
    }
  }
}
</script>

<style>
@import "https://unpkg.com/sakura.css/css/sakura.css";

.hidden {
  display: none;
}

label {
  display: inline;
}

[role="dialog"] {
  box-sizing: border-box;
  padding: 15px;
  border: 1px solid #000;
  background-color: #fff;
  min-height: 100vh;
}

@media screen and (min-width: 640px) {
  [role="dialog"] {
    position: absolute;
    top: 2rem;
    left: 50vw; /* move to the middle of the screen (assumes relative parent is the body/viewport) */
    transform: translateX(
      -50%
    ); /* move backwards 50% of this element's width */

    min-width: calc(640px - (15px * 2)); /* == breakpoint - left+right margin */
    min-height: auto;
    box-shadow: 0 19px 38px rgb(0 0 0 / 12%), 0 15px 12px rgb(0 0 0 / 22%);
  }
}

.dialog_label {
  text-align: center;
}

.dialog_form {
  margin: 15px;
}

.dialog_form .label_text {
  box-sizing: border-box;
  padding-right: 0.5em;
  display: inline-block;
  font-size: 16px;
  font-weight: bold;
  width: 30%;
  text-align: right;
}

.dialog_form .label_info {
  box-sizing: border-box;
  padding-right: 0.5em;
  font-size: 12px;
  width: 30%;
  text-align: right;
  display: inline-block;
}

.dialog_form_item {
  margin: 10px 0;
  font-size: 0;
}

.dialog_form_item .wide_input {
  box-sizing: border-box;
  max-width: 70%;
  width: 27em;
}

.dialog_form_item .city_input {
  box-sizing: border-box;
  max-width: 70%;
  width: 17em;
}

.dialog_form_item .state_input {
  box-sizing: border-box;
  max-width: 70%;
  width: 15em;
}

.dialog_form_item .zip_input {
  box-sizing: border-box;
  max-width: 70%;
  width: 9em;
}

.dialog_form_actions {
  text-align: right;
  padding: 0 20px 20px;
}

.dialog_close_button {
  float: right;
  position: absolute;
  top: 10px;
  left: 92%;
  height: 25px;
}
</style>
