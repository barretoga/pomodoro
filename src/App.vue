<script setup lang="ts"></script>

<template>
  <div class="w-full h-full sm:px-0 px-2 min-h-screen flex flex-col items-center">
    <div class="flex justify-center w-full mt-[2rem] mb-[3rem]">
      <h1 class="text-4xl">Pomodoro</h1>
    </div>

    <div class="grid sm:grid-cols-2 gap-4 w-full max-w-[80rem] h-full max-h-[40rem]">
      <div class="flex flex-col h-[18rem] bg-neutral p-5 rounded-md">
        <div class="flex justify-start">
          <h2 class="text-xl text-white">Timer</h2>
        </div>
        <div class="flex justify-center items-center h-full">
          <Timer />
        </div>
      </div>

      <div class="flex flex-col bg-neutral p-5 rounded-md">
        <div class="flex justify-start">
          <h2 class="text-xl text-white">To-Do</h2>
        </div>
        <div class="flex justify-center items-center">
          <Todo />
        </div>
      </div>
    </div>
  </div>
</template>
