<template>
  <fieldset>
    <legend>{{legend}}</legend>
    <div class="framing-form-row">
      <label>{{label}}</label>
      <div>
        <input
          type="number"
          step="0.1"
          v-model="val"
          @change="change($event.target.value)"
          :aria-label="legend"
        />
        cm
      </div>
    </div>
    <input
      type="range"
      step="0.1"
      :min="min / 10"
      :max="max / 10"
      v-model="val"
      @input="change($event.target.value)"
      aria-hidden="true"
    />
  </fieldset>
</template>

<script>
export default {
  name: "WidthSlider",
  props: {
    min: Number,
    max: Number,
    value: Number,
    label: String
  },
  computed: {
    legend() {
      return this.label + " Width";
    }
  },
  data: function () {
    return {
      val: this.value / 10,
    };
  },
  methods: {
    change(val) {
      val = val * 10;
      if(val < this.min){
        val = this.min;
      } else if(val > this.max) {
        val = this.max;
      }
      this.$emit('input', Math.trunc(val));
    }
  }
};
</script>

<style>

input[type="number"] {
  -moz-appearance: textfield;
  width: 2.5em;
  height: 1em;
  font-family: inherit;
  font-size: inherit;
  text-align: right;
  background-color: var(--bg-color);
  border: none;
  padding: 3px;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

/*
Styling the range input element is very tricky.
If you want to know more, see
https://css-tricks.com/sliding-nightmare-understanding-range-input
*/
input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  width: 100%;
  height: 0.5rem;
  background: transparent;
  background-color: var(--bg-color);
  border-radius: 0.5rem;
  margin: 1rem 0;
}

::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 24px;
  height: 24px;
  border-style: none;
  border-radius: 50%;
  background-color: white;
  box-shadow: 1px 1px 3px;
  margin-top: -2px;
  color: rgb(144,144,144);
}

::-moz-range-thumb {
  width: 24px;
  height: 24px;
  border-style: none;
  border-radius: 50%;
  background-color: white;
  box-shadow: 1px 1px 3px;
  color: rgb(144,144,144);
}

input[type="range"]:focus {
  outline: none;
}
</style>

<style scoped>
fieldset {
  border: none;
  min-width: auto;
  border-bottom: 1px solid var(--bg-color);
  padding: 15px 20px 15px 20px;
  margin: 0;
}

/* legend is necessary for accessibility, but we don't want to show it */
fieldset legend {
  position: absolute;
  clip: rect(0 0 0 0);
}

.framing-form-row {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  line-height: 1.5em;
}
</style>
