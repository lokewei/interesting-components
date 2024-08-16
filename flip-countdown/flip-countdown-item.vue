<template>
  <ul class="relative leading-none rounded-md" :class="{'w-7 h-11 md:w-8 md:h-14 text-8 md:text-10': size === 'large', 'text-8 w-7 h-11': size === 'normal'}">
    <li v-for="(num, index) in allNumbers" class="absolute inset-0" :class="{ 'z-10': index === 0, 'z-20': index === 1, 'z-30': play && index === 0, 'z-20 animate-[asd_.5s_.5s_linear_both]': play && index === 1 }" style="perspective:12rem;">
      <!-- <div class="left-10 absolute text-sm whitespace-nowrap" :class="{ 'top-0': index === 0, 'bottom-0': index === 1 }">{{ play }} {{ index }}</div> -->
      <!-- index=0:before index=1:active -->
      <!-- up -->
      <div class="absolute z-10 start-0 w-full h-1/2 rounded-t-md overflow-hidden top-0 origin-bottom transition-transform" :class="{'animate-[flip-up_.5s_linear_both] z-20': play && index === 0}">
        <!-- shadow -->
        <div class="absolute w-full h-full z-20" :class="{'bg-gradient-to-b from-dark-33/10 to-dark-33/100': play, 'animate-[shadow-show_.5s_linear_both]': play && index === 0, 'animate-[shadow-hide.5s_.3s_linear_both]': play && index === 1}"></div>
        <!-- inn -->
        <div class="absolute start-0 z-10 w-full h-[200%] flex justify-center items-center text-center top-0" :class="{'text-white bg-dark-66': theme === 'dark', 'text-dark-33 bg-white': theme === 'white'}">{{ num }}</div>
      </div>
      <!-- splitter -->
      <div class="absolute start-0 top-1/2 z-50 w-full bg-dark-33 h-px -translate-y-full"></div>
      <!-- down -->
      <div class="absolute z-10 start-0 w-full h-1/2 rounded-b-md overflow-hidden bottom-0 origin-top transition-transform" :class="{'animate-[flip-down_.5s_.5s_linear_both] z-20': play && index === 1}">
        <!-- shadow -->
        <div class="absolute w-full h-full z-20" :class="{'bg-gradient-to-b from-dark-33/100 to-dark-33/10': play, 'animate-[shadow-show_.5s_linear_both]': play && index === 0, 'animate-[shadow-hide.5s_.3s_linear_both]': play && index === 1}"></div>
        <!-- inn -->
        <div class="absolute start-0 z-10 w-full h-[200%] flex justify-center items-center text-center bottom-0" :class="{'text-white bg-dark-66': theme === 'dark', 'text-dark-33 bg-white': theme === 'white'}">{{ num }}</div>
        <!-- cover -->
        <div class="absolute inset-0 z-30 bg-gray-99/25"></div>
      </div>
    </li>
  </ul>
</template>

<script setup lang="ts">
const props = withDefaults(defineProps<{
  size?: 'normal' | 'large',
  theme?: 'dark' | 'white',
  num?: number
}>(), {
  num: 0,
  theme: 'dark',
  size: 'normal'
});

const newNum = ref(props.num);
const allNumbers = ref([props.num, newNum.value]);
const play = ref(false);

if (process.client) {
  watch(() => props.num, async (newVal, oldVal) => {
  newNum.value = newVal;
  allNumbers.value = [oldVal, newVal];
  play.value = true;

  window.setTimeout(() => {
    play.value = false;
  }, 1000);
});
}
</script>

<style>
@keyframes asd {
  0% {
    z-index: 20;
  }

  5% {
    z-index: 40;
  }

  100% {
    z-index: 40;
  }
}

@keyframes flip-down {
  0% {
    transform: rotateX(90deg);
  }

  100% {
    transform: rotateX(0deg);
  }
}

@keyframes flip-up {
  0% {
    transform: rotateX(0deg);
  }

  100% {
    transform: rotateX(-90deg);
  }
}

@keyframes shadow-show {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}

@keyframes shadow-hide {
  0% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}
</style>