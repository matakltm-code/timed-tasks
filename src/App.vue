<template>
  <div>
    <header
      class="sticky top-0 z-40 flex h-16 shrink-0 items-center gap-x-4 border-b border-gray-200 bg-white px-4 shadow-sm sm:gap-x-6 sm:px-6 lg:px-8">
      <div class="max-w-5xl mx-auto flex flex-1 gap-x-4 self-stretch lg:gap-x-6 ">
        <form class="relative flex flex-1" action="#" method="GET">
          <label for="search-field" class="sr-only">Search</label>
          <MagnifyingGlassIcon class="pointer-events-none absolute inset-y-0 left-0 h-full w-5 text-gray-400"
            aria-hidden="true" />
          <input id="search-field"
            class="block size-full border-0 border-green-500 py-0 pl-8 pr-0 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm"
            placeholder="Search..." type="search" name="search" />
        </form>
        <div class="flex items-center gap-x-4 lg:gap-x-6">
          <div class="shrink-0">
            <button @click="addTask" type="button"
              class="relative inline-flex items-center gap-x-1.5 rounded-md bg-indigo-500 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-400 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">
              <PlusIcon class="-ml-0.5 size-5" aria-hidden="true" />
              New Task
            </button>
          </div>
        </div>
      </div>
    </header>
    <main class="bg-white mx-auto max-w-md sm:max-w-5xl px-4 py-10 sm:px-6 lg:px-8 lg:py-6 ">
      <section>
        <Task v-for="task in tasks" :key="task.id" :taskText="task.taskText" />
      </section>
      <section v-if="true" class="mx-auto max-w-md sm:max-w-5xl">
        <div>
          <div class="text-center">
            <svg class="mx-auto size-12 text-gray-400" stroke="currentColor" fill="none" viewBox="0 0 48 48"
              aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M8 14v20c0 4.418 7.163 8 16 8 1.381 0 2.721-.087 4-.252M8 14c0 4.418 7.163 8 16 8s16-3.582 16-8M8 14c0-4.418 7.163-8 16-8s16 3.582 16 8m0 0v14m0-4c0 4.418-7.163 8-16 8S8 28.418 8 24m32 10v6m0 0v6m0-6h6m-6 0h-6" />
            </svg>
            <h2 class="mt-2 text-sm font-semibold text-gray-900">Timed Task Examples</h2>
            <!-- <p class="mt-1 text-sm text-gray-500">You can use any of this timed tasks.</p> -->
          </div>
        </div>
        <div class="mt-10">
          <h3 class="text-sm font-medium text-gray-400">Recommended way of timed task writing</h3>
          <ul role="list" class="mt-4 grid grid-cols-1 gap-4 sm:grid-cols-2">
            <li v-for="item in exampleTasks">
              <button @click="addExampleTask(item)" type="button"
                class="group flex w-full items-center justify-between space-x-3 rounded-full border border-gray-200 p-2 text-left shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
                <span class="flex min-w-0 flex-1 items-center space-x-3">
                  <span class="ml-10 block min-w-0 flex-1">
                    <span class="block  text-sm font-medium text-gray-600 select-text">{{ item.text }}</span>
                  </span>
                </span>
                <span class="inline-flex size-10 shrink-0 items-center justify-center">
                  <svg class="size-5 text-gray-400 group-hover:text-gray-500" viewBox="0 0 20 20" fill="currentColor"
                    aria-hidden="true" data-slot="icon">
                    <path
                      d="M10.75 4.75a.75.75 0 0 0-1.5 0v4.5h-4.5a.75.75 0 0 0 0 1.5h4.5v4.5a.75.75 0 0 0 1.5 0v-4.5h4.5a.75.75 0 0 0 0-1.5h-4.5v-4.5Z" />
                  </svg>
                </span>
              </button>
            </li>
          </ul>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { PlusIcon } from '@heroicons/vue/24/outline';
import { MagnifyingGlassIcon } from '@heroicons/vue/20/solid';

import Task from './components/Task.vue';

const tasks = ref([]);

const addTask = () => {
  console.log('Please add new task because he clicked enter');
  tasks.value.push({ id: Date.now(), taskText: '' });
};
const addExampleTask = (item) => {
  console.log(item);
  tasks.value.push({ id: Date.now(), taskText: item.text });
};

const exampleTasks = [
  { id: 1, text: "Complete Vue project documentation - 45 minutes" },
  { id: 2, text: "Email the client for project updates" },
  { id: 3, text: "Review codebase for optimization - 1 hour" },
  { id: 4, text: "Prepare slides for the team meeting" },
  { id: 5, text: "Brainstorm new features for next release - 30 minutes" },
  { id: 6, text: "Schedule a call with the design team - 15 minutes" },
  { id: 7, text: "Research competitor features" },
  { id: 8, text: "Write weekly progress report - 2 hours" },
  { id: 9, text: "Clean up project repo and archive old branches" },
  { id: 10, text: "Plan tasks for next sprint - 1 hour 30 minutes" },
];

</script>