<script>
  import { wrap } from './actions';
  import { createClassname } from './helpers';
  import { MDCSlider } from '@material/slider';

  export let discrete = false,
    disabled = false,
    value = 0,
    min = 0,
    max = 100,
    displayMarkers = false,
    step = 0;

  const mdc = wrap(MDCSlider, {
    initialize(slider) {
      slider.listen('MDCSlider:input', () => {
        value = slider.value;
      });
      for (const e of ['closed', 'opened']) {
        window.addEventListener(`MDCDrawer:${e}`, () => slider.layout())
      }
      slider.layout();
    },
    update(slider, { value: newValue, disabled, max, min, step }) {
      slider.value = newValue;
      slider.disabled = disabled;
      slider.min = min;
      slider.step = step;
      slider.max = max;
    }
  })
  $: className = createClassname('slider', { discrete, displayMarkers });
</script>

<div use:mdc={{ value, disabled, max, min, step }}
  class={className}
  tabindex="0" role="slider"
  aria-valuemin={min} aria-valuemax={max}
  aria-valuenow={value} data-step={step}
  aria-disabled={disabled}>
    <div class="mdc-slider__track-container">
      <div class="mdc-slider__track"></div>
      {#if discrete && displayMarkers}
        <div class="mdc-slider__track-marker-container"></div>
      {/if}
    </div>
    <div class="mdc-slider__thumb-container">
      {#if discrete}
        <div class="mdc-slider__pin">
          <span class="mdc-slider__pin-value-marker"></span>
        </div>
      {/if}
      <svg class="mdc-slider__thumb" width="21" height="21">
        <circle cx="10.5" cy="10.5" r="7.875"></circle>
      </svg>
      <div class="mdc-slider__focus-ring"></div>
    </div>
 </div>
