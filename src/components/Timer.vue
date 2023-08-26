<script setup lang="ts">
import { computed, ref } from "vue";
import { Icon } from "@iconify/vue";

interface Props {
  class?: string;
}

const props = defineProps<Props>();

let workTime = 25 * 60;
let intervalId: number | undefined = undefined;
let paused = ref(true);
const time = ref(workTime);

function formatSeconds(seconds: number) {
  const remainingSeconds = seconds % 60;
  return `${remainingSeconds
    .toString()
    .padStart(2, "0")}`;
}

function formatMinutes(seconds: number) {
  const minutes = Math.floor(seconds / 60);
  return `${minutes
    .toString()
    .padStart(2, "0")}`;
}

const formattedMinutes = computed(() => {
  return formatMinutes(time.value);
});

const formattedSeconds = computed(() => {
  return formatSeconds(time.value);
});

function play() {
  if (paused.value) {
    paused.value = false;
    intervalId = setInterval(() => {
      time.value--;
      if (time.value === 0) {
        clearInterval(intervalId);
        paused.value = true;
      }
    }, 1000);
  }
}

function stop() {
  clearInterval(intervalId);
  time.value = workTime;
  paused.value = true;
}

function pause() {
  clearInterval(intervalId);
  paused.value = true;
}

function changeTime(newMinutes: number) {
  const newTime = newMinutes * 60;
  workTime = newTime;
  time.value = newTime;
  paused.value = true;
  clearInterval(intervalId);
}
</script>

<template>
  <div :class="props.class">
    <div class="flex justify-center items-center">
      <div class="flex flex-col items-center">
        <div class="join my-2">
          <input
            class="join-item btn text-xs"
            type="radio"
            name="options"
            aria-label="Pomodoro"
            @click="changeTime(25)"
            checked
          />
          <input
            class="join-item btn text-xs"
            type="radio"
            name="options"
            aria-label="Short break"
            @click="changeTime(5)"
          />
          <input
            class="join-item btn text-xs"
            type="radio"
            name="options"
            aria-label="Long break"
            @click="changeTime(10)"
          />
        </div>
        <div class="flex justify-center items-center">
          <div className="grid grid-flow-col gap-5 text-center auto-cols-max">
            <div className="flex flex-col p-2 bg-primary rounded-box text-neutral-content">
              <span className="countdown font-mono text-5xl">
                <span :style="`--value:${formattedMinutes};`"></span>
              </span>
              min
            </div> 
            <div className="flex flex-col p-2 bg-primary rounded-box text-neutral-content">
              <span className="countdown font-mono text-5xl">
                <span :style="`--value:${formattedSeconds};`"></span>
              </span>
              sec
            </div>
          </div>
        </div>
        <div class="flex justify-center items-center mt-2">
          <button
            class="bg-primary text-white hover:bg-slate-100 hover:text-black transition-all duration-300 rounded-md px-5 mx-1 py-2"
            @click="play"
          >
            <Icon icon="ci:play" class="w-6 h-6" />
          </button>
          <button
            class="bg-primary text-white hover:bg-slate-100 hover:text-black transition-all duration-300 rounded-md px-5 mx-1 py-2"
            @click="pause"
          >
            <Icon icon="ci:pause" class="w-6 h-6" />
          </button>
          <button
            class="bg-primary text-white hover:bg-slate-100 hover:text-black transition-all duration-300 rounded-md px-5 mx-1 py-2"
            @click="stop"
          >
            <Icon icon="ci:stop" class="w-6 h-6" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
