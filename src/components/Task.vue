<template>
    <div class="flex flex-col">
        <!-- <div v-if="false" class="flex items-center space-x-5 font-bold text-sm">
            <span v-if="countdown" class="text-red-500 ml-15">{{ countdownDisplay }}</span>
        </div>
        <div v-if="false" class="flex items-center space-x-4 p-2">
            <svg v-if="countdown && countdown > 0" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                stroke-width="1.5" stroke="currentColor" class="w-6 h-6 bg-green-500 text-white rounded-full">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5" />
            </svg>

            <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="w-8 h-8" :class="[countdown ? 'block' : 'hidden']">
                <path stroke-linecap="round" stroke-linejoin="round"
                    d="m12.75 15 3-3m0 0-3-3m3 3h-7.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>

            <input v-model="taskText" @input="enforceLimit" @keyup.enter="checkForTime" type="text"
                placeholder="Write a timed task"
                class="flex-1 px-2 py-1 bg-transparent outline-none font-bold text-xl resize-none" />
            <span>{{ taskText.length }}/{{ taskTextLimit }}</span>
        </div> -->

        <div class="mb-8">
            <div class="pl-4 flex justify-between">
                <label for="task" class="block text-sm/6 font-medium text-red-600">{{ countdownDisplay }}</label>
                <span class="text-sm/6 text-gray-500" id="task-optional">{{ taskText.length }}/{{ taskTextLimit
                    }}</span>
            </div>
            <div class="">
                <!-- <input type="text" name="task" id="task"
                    class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm/6"
                    placeholder="Write a timed task" aria-describedby="task-optional" /> -->
                <div class="relative ">
                    <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
                        <span class="text-gray-500 sm:text-sm">
                            <PauseIcon v-if="countdown && countdown > 0"
                                class="w-4 h-4 text-white bg-green-400 rounded-full p-1 animate-ping"
                                aria-hidden="true" />
                            <PlayIcon v-else class="w-4 h-4" aria-hidden="true" />
                        </span>
                    </div>
                    <input v-model="taskText" @input="enforceLimit" @keyup.enter="checkForTime" type="text"
                        name="timed-task" id="timed-task"
                        class="block w-full rounded-md border-0 border-white py-1.5 pl-10 pr-12 text-gray-900 font-bold ring-1 ring-inset ring-white placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-white sm:text-sm/6  lg:text-lg/6"
                        placeholder="Write a timed task" aria-describedby="timed-task-currency" />
                    <div class="absolute inset-y-0 right-0 flex py-1.5 pr-1.5">
                        <kbd
                            class="inline-flex items-center rounded border border-gray-200 px-1 font-sans text-xs text-gray-400">⌘K</kbd>
                    </div>
                    <!-- <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center pr-3">
                        <span class="text-gray-500 sm:text-sm" id="timed-task-currency">USD</span>
                    </div> -->
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { PlayIcon, PauseIcon } from '@heroicons/vue/20/solid';
import { ref, computed, onUnmounted } from 'vue';

const props = defineProps({
    taskText: {
        type: String,
        default: '',
    }
});

const taskTextLimit = 100;
const taskText = ref(props.taskText);
const countdown = ref(null);

let countdownInterval;

const enforceLimit = () => {
    if (taskText.value.length > taskTextLimit) {
        taskText.value = taskText.value.slice(0, taskTextLimit);
    }
};

const checkForTime = () => {
    // Match all time units in the format "number + unit"
    const timeMatches = taskText.value.match(/(\d+)\s*(seconds?|secs?|minutes?|mins?|hours?|hrs?|days?|weeks?)/gi);
    if (timeMatches) {
        let totalSeconds = 0;

        timeMatches.forEach((match) => {
            const [_, amount, unit] = match.match(/(\d+)\s*(seconds?|secs?|minutes?|mins?|hours?|hrs?|days?|weeks?)/i);
            const timeAmount = parseInt(amount);

            if (/h(ours?|rs?)/i.test(unit)) {
                totalSeconds += timeAmount * 3600; // Convert hours to seconds
            } else if (/m(inutes?|ins?)/i.test(unit)) {
                totalSeconds += timeAmount * 60; // Convert minutes to seconds
            } else if (/s(econds?|ecs?)/i.test(unit)) {
                totalSeconds += timeAmount; // Seconds
            } else if (/d(ays?)/i.test(unit)) {
                totalSeconds += timeAmount * 86400; // Convert days to seconds
            } else if (/w(eeks?)/i.test(unit)) {
                totalSeconds += timeAmount * 604800; // Convert weeks to seconds
            }
        });

        startCountdown(totalSeconds);
    }
};

const startCountdown = (duration) => {
    countdown.value = duration;
    clearInterval(countdownInterval);
    countdownInterval = setInterval(() => {
        if (countdown.value > 0) {
            countdown.value -= 1;
        } else {
            clearInterval(countdownInterval);
        }
    }, 1000);
};

const countdownDisplay = computed(() => {
    if (countdown.value === null) return '';

    let remainingTime = countdown.value;

    const weeks = Math.floor(remainingTime / 604800); // Calculate weeks
    remainingTime %= 604800;

    const days = Math.floor(remainingTime / 86400); // Calculate days
    remainingTime %= 86400;

    const hours = Math.floor(remainingTime / 3600); // Calculate hours
    remainingTime %= 3600;

    const minutes = Math.floor(remainingTime / 60); // Calculate minutes
    const seconds = remainingTime % 60; // Remaining seconds

    const weekStr = weeks > 0 ? `${weeks} week${weeks > 1 ? 's' : ''}, ` : '';
    const dayStr = days > 0 ? `${days} day${days > 1 ? 's' : ''}, ` : '';
    const timeStr = `${hours.toString().padStart(2, '0')}:${minutes
        .toString()
        .padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;

    return `${weekStr}${dayStr}${timeStr}`.replace(/, $/, ''); // Trim trailing comma if no weeks/days
});


onUnmounted(() => clearInterval(countdownInterval));
</script>