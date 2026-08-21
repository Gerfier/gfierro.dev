<script setup>
import { onMounted, ref } from "vue";

const links = [
  { href: "#about", label: "About" },
  { href: "#experience", label: "Experience" },
  { href: "#skills", label: "Skills" },
  { href: "#projects", label: "Projects" },
  { href: "#contact", label: "Contact" },
];

const menuOpen = ref(false);
const isDark = ref(true);
const scrolled = ref(false);

function toggleTheme() {
  isDark.value = !isDark.value;
  document.documentElement.classList.toggle("dark", isDark.value);
  localStorage.setItem("theme", isDark.value ? "dark" : "light");
}

function closeMenu() {
  menuOpen.value = false;
}

onMounted(() => {
  isDark.value = document.documentElement.classList.contains("dark");
  const onScroll = () => {
    scrolled.value = window.scrollY > 8;
  };
  onScroll();
  window.addEventListener("scroll", onScroll, { passive: true });
});
</script>

<template>
  <header
    class="fixed inset-x-0 top-0 z-50 transition-colors duration-300"
    :class="scrolled ? 'bg-ink-50/80 dark:bg-ink-950/80 backdrop-blur-md border-b border-ink-200/60 dark:border-ink-800/60' : ''"
  >
    <nav class="mx-auto flex max-w-5xl items-center justify-between px-6 py-4">
      <a href="#top" class="font-mono text-sm font-semibold tracking-tight text-ink-900 dark:text-ink-100">
        gfierro<span class="text-accent-500">.</span>dev
      </a>

      <ul class="hidden items-center gap-8 md:flex">
        <li v-for="link in links" :key="link.href">
          <a
            :href="link.href"
            class="text-sm font-medium text-ink-600 transition-colors hover:text-accent-500 dark:text-ink-300 dark:hover:text-accent-400"
          >
            {{ link.label }}
          </a>
        </li>
      </ul>

      <div class="flex items-center gap-3">
        <button
          @click="toggleTheme"
          type="button"
          aria-label="Toggle color theme"
          class="grid h-9 w-9 place-items-center rounded-full border border-ink-200 text-ink-600 transition-colors hover:border-accent-500 hover:text-accent-500 dark:border-ink-700 dark:text-ink-300 dark:hover:border-accent-400 dark:hover:text-accent-400"
        >
          <svg v-if="isDark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" class="h-4 w-4">
            <circle cx="12" cy="12" r="4" />
            <path d="M12 2v2M12 20v2M4.93 4.93l1.41 1.41M17.66 17.66l1.41 1.41M2 12h2M20 12h2M4.93 19.07l1.41-1.41M17.66 6.34l1.41-1.41" />
          </svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" class="h-4 w-4">
            <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" />
          </svg>
        </button>

        <button
          @click="menuOpen = !menuOpen"
          type="button"
          aria-label="Toggle navigation menu"
          class="grid h-9 w-9 place-items-center rounded-full border border-ink-200 text-ink-600 md:hidden dark:border-ink-700 dark:text-ink-300"
        >
          <svg v-if="!menuOpen" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" class="h-4 w-4">
            <path stroke-linecap="round" d="M4 7h16M4 12h16M4 17h16" />
          </svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" class="h-4 w-4">
            <path stroke-linecap="round" d="M6 6l12 12M18 6L6 18" />
          </svg>
        </button>
      </div>
    </nav>

    <Transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="opacity-0 -translate-y-2"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100 translate-y-0"
      leave-to-class="opacity-0 -translate-y-2"
    >
      <ul
        v-if="menuOpen"
        class="mx-4 mb-4 flex flex-col gap-1 rounded-2xl border border-ink-200 bg-ink-50/95 p-3 shadow-lg shadow-ink-900/5 backdrop-blur-md md:hidden dark:border-ink-800 dark:bg-ink-900/95"
      >
        <li v-for="link in links" :key="link.href">
          <a
            :href="link.href"
            @click="closeMenu"
            class="block rounded-xl px-4 py-2.5 text-sm font-medium text-ink-700 transition-colors hover:bg-accent-500/10 hover:text-accent-500 dark:text-ink-200"
          >
            {{ link.label }}
          </a>
        </li>
      </ul>
    </Transition>
  </header>
</template>
