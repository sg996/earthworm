<template>
  <div class="text-center mb-20 mt-10">
    <div class="text-5xl mb-3 text-fuchsia-500 dark:text-gray-50">
      {{ word }}
      <svg
        class="w-7 h-7 inline-block ml-1 cursor-pointer"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        @click="handlePlaySound"
      >
        <path
          d="M342.4 384H128v256h214.4L576 826.8V197.2L342.4 384zM64 320h256L640 64v896L320 704H64V320z m640 256h256v-64H704v64z m16.8 159.5l181 181 45.3-45.3-181-181-45.3 45.3z m33.9-343.9l181-181-45.3-45.3-181 181 45.3 45.3z"
          fill="#666666"
        ></path>
      </svg>
    </div>
    <div class="text-2xl text-slate-600">{{ soundmark }}</div>
    <button
      class="border-solid border-2 border-slate-400 bg-slate-100 dark:bg-fuchsia-500 rounded-lg mt-8 mb-11 indent-1 h-10 text-2xl pl-10 pr-10 hover:bg-slate-200"
      @click="handleToNextStatement"
    >
      next
    </button>
  </div>
</template>

<script setup lang="ts">
import { useCoursesStore } from "~/store/courses";
import { useEnglishSound } from "~/composables/useEnglishSound";

const emit = defineEmits(["nextQuestion"]);

const { currentStatement, toNextStatement } = useCoursesStore();

const soundmark = ref("");
const word = ref("");

onMounted(() => {
  play();
});

const { play } = useEnglishSound(word);

watchEffect(() => {
  soundmark.value = currentStatement?.soundmark!;
  word.value = currentStatement?.english!;
});

async function handleToNextStatement() {
  toNextStatement();
  emit("nextQuestion");
}

function handlePlaySound() {
  play();
}

// TODO 后续处理
// useKeyboardShortcut()
function useKeyboardShortcut() {
  const handleKeyDown = (event: KeyboardEvent) => {
    if (event.key === "Enter") {
      console.log("enter");
      //       handleToNextStatement();
    }
  };

  onMounted(() => {
    document.addEventListener("keydown", handleKeyDown);
  });

  onUnmounted(() => {
    document.removeEventListener("keydown", handleKeyDown);
  });
}
</script>