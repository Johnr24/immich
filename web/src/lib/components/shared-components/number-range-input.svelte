<script lang="ts">
  import { clamp } from 'lodash-es';
  import type { ClipboardEventHandler } from 'svelte/elements';

  interface Props {
    id: string;
    min: number;
    max: number;
    step?: number | string;
    required?: boolean;
    value?: number;
    onInput: (value: number | null) => void;
    onPaste?: ClipboardEventHandler<HTMLInputElement>;
  }

  let {
    id,
    min,
    max,
    step = 'any',
    required = true,
    value = $bindable(),
    onInput,
    onPaste = undefined,
  }: Props = $props();

  const oninput = (newValue: number) => {
    value = clamp(newValue, min, max);
    onInput(value);
  };
</script>

<input
  type="number"
  class="immich-form-input w-full"
  {id}
  {min}
  {max}
  {step}
  {required}
  value
  oninput={({ currentTarget }) => oninput(currentTarget.valueAsNumber)}
  onpaste={onPaste}
/>
