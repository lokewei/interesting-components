<template>
  <!-- container -->
  <div>
    <div class="flex justify-center items-center gap-1">
      <!-- hours -->
      <FlipCountdownItem :size="size" :theme="theme" :num="Number(countDownTime.hrs.charAt(0))" />
      <FlipCountdownItem :size="size" :theme="theme" :num="Number(countDownTime.hrs.charAt(1))" />
      <div class="flex flex-col justify-center items-center gap-3 mx-1">
        <div class="w-1.5 h-1.5 rounded-full" :class="{'bg-dark-66': theme === 'dark', 'bg-white': theme === 'white'}"></div>
        <div class="w-1.5 h-1.5 rounded-full" :class="{'bg-dark-66': theme === 'dark', 'bg-white': theme === 'white'}"></div>
      </div>
      <!-- Minutes -->
      <FlipCountdownItem :size="size" :theme="theme" :num="Number(countDownTime.min.charAt(0))" />
      <FlipCountdownItem :size="size" :theme="theme" :num="Number(countDownTime.min.charAt(1))" />
      <div class="flex flex-col justify-center items-center gap-3 mx-1">
        <div class="w-1.5 h-1.5 rounded-full" :class="{'bg-dark-66': theme === 'dark', 'bg-white': theme === 'white'}"></div>
        <div class="w-1.5 h-1.5 rounded-full" :class="{'bg-dark-66': theme === 'dark', 'bg-white': theme === 'white'}"></div>
      </div>
      <!-- Seconds -->
      <FlipCountdownItem :size="size" :theme="theme" :num="Number(countDownTime.sec.charAt(0))" />
      <FlipCountdownItem :size="size" :theme="theme" :num="Number(countDownTime.sec.charAt(1))" />
    </div>
    <div class="flex justify-center items-center gap-6 mt-2 text-xs" :class="{'text-dark-66/80': theme === 'dark', 'text-white/50': theme === 'white'}">
      <div class="flex-1 text-center">{{ $t('hours') }}</div>
      <div class="flex-1 text-center">{{ $t('minutes') }}</div>
      <div class="flex-1 text-center">{{ $t('seconds') }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { showtime } from '@/common/utils';
const props = withDefaults(
  defineProps<{
    size?: 'normal' | 'large';
    theme?: 'dark' | 'white';
    endDate: string | Date;
  }>(),
  {
    size: 'normal',
    theme: 'dark'
  },
);
// 时间计算
// const initTime = showtime(props.endDate);
const countDownTime = ref<TShowTime>({
  days: '00',
  hrs: '00',
  min: '00',
  sec: '00',
  leftTime: 1
});
let timer: number | null;

onMounted(() => {
  timer = window.setInterval(() => {
    if (countDownTime.value.leftTime > 0) {
      countDownTime.value = showtime(props.endDate);
    } else {
      timer && clearInterval(timer);
      timer = null;
    }
  }, 1050);
});

onBeforeUnmount(() => {
  if (timer) {
    window.clearInterval(timer);
    timer = null;
  }
});
</script>