<script setup>
import { computed, ref } from "vue";

const projects = [
  {
    title: "Micro-frontend decoupling",
    company: "Virtana",
    description:
      "Refactored core Zenoss product features into standalone, reusable micro-frontends to support a strategic platform partnership, and migrated several pages to a shared query-criteria structure to unify data flow across APIs.",
    tags: ["Vue.js", "Micro-frontends", "GraphQL"],
    link: null,
    featured: true,
  },
  {
    title: "Inventory query builder",
    company: "Zenoss",
    description:
      "Designed and built a dynamic query builder for entity retrieval with query-saving, used as the primary inventory exploration tool across an IT monitoring cloud platform.",
    tags: ["Vue.js", "Vuetify", "GraphQL"],
    link: null,
  },
  {
    title: "Events & dashboard visualizations",
    company: "Zenoss",
    description:
      "Built an events page with graph and pie-chart visualizations for notifications, plus filterable dashboard tiles with gauge charts for event and metrics analysis.",
    tags: ["Vue.js", "Data viz", "Vuetify"],
    link: null,
  },
  {
    title: "Vue 2 → 3 platform migration",
    company: "Zenoss",
    description:
      "Co-led the migration from Vue 2 to Vue 3 across the product, moving the build from Webpack to Vite and state management from Vuex to Pinia — improving build speed and shrinking bundle size.",
    tags: ["Vue.js", "Vite", "Pinia"],
    link: null,
  },
];

const allTags = computed(() => ["All", ...new Set(projects.flatMap((p) => p.tags))]);
const activeTag = ref("All");

const filtered = computed(() =>
  activeTag.value === "All" ? projects : projects.filter((p) => p.tags.includes(activeTag.value))
);

const canTilt =
  typeof window !== "undefined" &&
  window.matchMedia("(pointer: fine)").matches &&
  !window.matchMedia("(prefers-reduced-motion: reduce)").matches;

function onCardMove(event) {
  if (!canTilt) return;
  const card = event.currentTarget;
  const rect = card.getBoundingClientRect();
  const px = (event.clientX - rect.left) / rect.width - 0.5;
  const py = (event.clientY - rect.top) / rect.height - 0.5;
  card.style.transform = `perspective(800px) rotateX(${py * -6}deg) rotateY(${px * 6}deg)`;
}

function onCardLeave(event) {
  event.currentTarget.style.transform = "";
}
</script>

<template>
  <div>
    <div class="flex flex-wrap gap-2">
      <button
        v-for="tag in allTags"
        :key="tag"
        @click="activeTag = tag"
        type="button"
        class="rounded-full px-4 py-1.5 text-sm font-medium transition-colors"
        :class="
          activeTag === tag
            ? 'bg-ink-950 text-white dark:bg-white dark:text-ink-950'
            : 'border border-ink-200 text-ink-600 hover:border-accent-500 hover:text-accent-600 dark:border-ink-700 dark:text-ink-300 dark:hover:border-accent-400 dark:hover:text-accent-400'
        "
      >
        {{ tag }}
      </button>
    </div>

    <TransitionGroup
      tag="div"
      class="mt-8 grid gap-5 sm:grid-cols-2"
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="opacity-0 translate-y-2"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition duration-150 ease-in absolute"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <article
        v-for="project in filtered"
        :key="project.title"
        @pointermove="onCardMove"
        @pointerleave="onCardLeave"
        class="gradient-border group rounded-2xl border border-ink-200 bg-ink-100/30 p-6 transition-[transform,border-color] duration-150 ease-out will-change-transform hover:border-accent-500/50 dark:border-ink-800 dark:bg-ink-900/30"
        :class="project.featured ? 'sm:col-span-2' : ''"
      >
        <p class="font-mono text-xs text-ink-500 dark:text-ink-400">{{ project.company }}</p>
        <h3 class="mt-1 text-lg font-bold text-ink-950 dark:text-white">{{ project.title }}</h3>
        <p class="mt-3 text-sm leading-relaxed text-ink-600 dark:text-ink-300">
          {{ project.description }}
        </p>
        <div class="mt-4 flex flex-wrap gap-2">
          <span
            v-for="tag in project.tags"
            :key="tag"
            class="rounded-full bg-ink-100 px-2.5 py-1 text-xs font-medium text-ink-600 dark:bg-ink-800 dark:text-ink-300"
          >
            {{ tag }}
          </span>
        </div>
      </article>
    </TransitionGroup>

    <p class="mt-8 text-sm text-ink-500 dark:text-ink-400">
      These summarize work from my professional roles under NDA — write-ups and screenshots for
      personal / open-source projects go here as I publish them.
    </p>
  </div>
</template>
