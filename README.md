# OTP Input for Vue

https://github.com/wobsoriano/vue-input-otp/assets/13049130/c5080f41-f411-4d38-aa57-d04d90c832c3

One-time passcode Input. Accessible & unstyled. Based on the [React version](https://github.com/guilhermerodz/input-otp) by [guilhermerodz](https://github.com/guilhermerodz).

## Installation

```bash
npm install vue-input-otp
```

## Usage

```vue
<script setup lang="ts">
import { OTPInput } from 'vue-input-otp'
import Slot from './Slot.vue'

const input = ref('123456')
</script>

<template>
  <form>
    <OTPInput v-slot="{ slots }" v-model="input" :maxlength="6">
      <!-- slots -->
    </OTPInput>
  </form>
</template>
```

## Default example

The example below uses `tailwindcss`, `shadcn-vue`, `tailwind-merge` and `clsx`:

```vue
<script setup lang="ts">
import { OTPInput } from 'vue-input-otp'
import Slot from './Slot.vue'
</script>

<template>
  <OTPInput
    v-slot="{ slots }"
    :maxlength="6"
    container-class="group flex items-center has-[:disabled]:opacity-30"
  >
    <div class="flex">
      <Slot v-for="(slot, idx) in slots.slice(0, 3)" v-bind="slot" :key="idx" />
    </div>

    <!-- Fake Dash. Inspired by Stripe's MFA input. -->
    <div class="flex w-10 justify-center items-center">
      <div class="w-3 h-1 rounded-full bg-border" />
    </div>

    <div class="flex">
      <Slot v-for="(slot, idx) in slots.slice(3)" v-bind="slot" :key="idx" />
    </div>
  </OTPInput>
</template>
```

```vue
<script setup lang="ts">
import type { SlotProps } from 'vue-input-otp'
import { cn } from '$lib/utils'

defineProps<SlotProps>()
</script>

<template>
  <div
    :class="cn(
      'relative w-10 h-14 text-[2rem]',
      'flex items-center justify-center',
      'transition-all duration-300',
      'border-border border-y border-r first:border-l first:rounded-l-md last:rounded-r-md',
      'group-hover:border-accent-foreground/20 group-focus-within:border-accent-foreground/20',
      'outline outline-0 outline-accent-foreground/20',
      { 'outline-4 outline-accent-foreground': isActive },
    )"
  >
    <div v-if="char !== null">
      {{ char }}
    </div>

    <!-- Emulate a Fake Caret -->
    <div v-if="char === null && isActive" class="absolute pointer-events-none inset-0 flex items-center justify-center animate-caret-blink">
      <div class="w-px h-8 bg-white" />
    </div>
  </div>
</template>
```

```ts
// tailwind.config.ts for the blinking caret animation.
// Small utility to merge class names.
import { clsx } from 'clsx'
import { twMerge } from 'tailwind-merge'

import type { ClassValue } from 'clsx'

const config = {
  theme: {
    extend: {
      keyframes: {
        'caret-blink': {
          '0%,70%,100%': { opacity: '1' },
          '20%,50%': { opacity: '0' },
        },
      },
      animation: {
        'caret-blink': 'caret-blink 1.2s ease-out infinite',
      },
    },
  },
}

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

## How it works

There's currently no native OTP/2FA/MFA input in HTML, which means people are either going with 1. a simple input design or 2. custom designs like this one. This library works by rendering an invisible input as a sibling of the slots, contained by a `relative`ly positioned parent (the container root called OTPInput).

## API Reference

### OTPInput

The root container. Define settings for the input via props. Then, pass in child elements to create the slots.

## License

MIT
