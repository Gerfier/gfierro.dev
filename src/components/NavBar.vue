<script setup>
import { onBeforeUnmount, onMounted, ref } from "vue";

const links = [
  { href: "#about", label: { en: "About", es: "Acerca de" } },
  { href: "#experience", label: { en: "Experience", es: "Experiencia" } },
  { href: "#skills", label: { en: "Skills", es: "Habilidades" } },
  { href: "#projects", label: { en: "Projects", es: "Proyectos" } },
  { href: "#contact", label: { en: "Contact", es: "Contacto" } },
];

const menuOpen = ref(false);
const isDark = ref(true);
const scrolled = ref(false);
const activeSection = ref("");
const themeBtn = ref(null);
const lang = ref("en");

let sectionObserver;

function toggleTheme(event) {
  const next = !isDark.value;
  const reduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;

  const apply = () => {
    isDark.value = next;
    document.documentElement.classList.toggle("dark", next);
    localStorage.setItem("theme", next ? "dark" : "light");
  };

  if (reduceMotion || !document.startViewTransition) {
    apply();
    return;
  }

  const rect = (event?.currentTarget ?? themeBtn.value)?.getBoundingClientRect();
  if (rect) {
    document.documentElement.style.setProperty("--toggle-x", `${rect.left + rect.width / 2}px`);
    document.documentElement.style.setProperty("--toggle-y", `${rect.top + rect.height / 2}px`);
  }

  document.startViewTransition(apply);
}

function toggleLang() {
  const next = lang.value === "es" ? "en" : "es";
  lang.value = next;
  document.documentElement.setAttribute("data-lang", next);
  document.documentElement.setAttribute("lang", next);
  localStorage.setItem("lang", next);
}

function closeMenu() {
  menuOpen.value = false;
}

onMounted(() => {
  isDark.value = document.documentElement.classList.contains("dark");
  lang.value = document.documentElement.getAttribute("data-lang") === "es" ? "es" : "en";

  const onScroll = () => {
    scrolled.value = window.scrollY > 8;
  };
  onScroll();
  window.addEventListener("scroll", onScroll, { passive: true });

  const sections = links
    .map((link) => document.querySelector(link.href))
    .filter(Boolean);

  sectionObserver = new IntersectionObserver(
    (entries) => {
      for (const entry of entries) {
        if (entry.isIntersecting) {
          activeSection.value = `#${entry.target.id}`;
        }
      }
    },
    { rootMargin: "-45% 0px -50% 0px" }
  );
  sections.forEach((section) => sectionObserver.observe(section));

  onBeforeUnmount(() => {
    window.removeEventListener("scroll", onScroll);
    sectionObserver?.disconnect();
  });
});
</script>

<template>
  <div
    class="scroll-progress fixed inset-x-0 top-0 z-[60] h-0.5 origin-left bg-gradient-to-r from-accent-500 to-violet-500"
    aria-hidden="true"
  ></div>

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
            class="relative text-sm font-medium transition-colors"
            :class="
              activeSection === link.href
                ? 'text-accent-600 dark:text-accent-400'
                : 'text-ink-600 hover:text-accent-500 dark:text-ink-300 dark:hover:text-accent-400'
            "
          >
            {{ link.label[lang] }}
            <span
              v-if="activeSection === link.href"
              class="absolute -bottom-1.5 left-0 h-0.5 w-full rounded-full bg-accent-500"
            />
          </a>
        </li>
      </ul>

      <div class="flex items-center gap-3">
        <button
          @click="toggleLang"
          type="button"
          :aria-label="lang === 'es' ? 'Cambiar a inglés' : 'Switch to Spanish'"
          class="grid h-9 w-9 place-items-center rounded-full border border-ink-200 font-mono text-xs font-semibold text-ink-600 transition-colors hover:border-accent-500 hover:text-accent-500 dark:border-ink-700 dark:text-ink-300 dark:hover:border-accent-400 dark:hover:text-accent-400"
        >
          {{ lang === "es" ? "EN" : "ES" }}
        </button>

        <button
          ref="themeBtn"
          @click="toggleTheme"
          type="button"
          :aria-label="lang === 'es' ? 'Alternar tema de color' : 'Toggle color theme'"
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
          :aria-label="lang === 'es' ? 'Alternar menú de navegación' : 'Toggle navigation menu'"
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
            class="block rounded-xl px-4 py-2.5 text-sm font-medium transition-colors"
            :class="
              activeSection === link.href
                ? 'bg-accent-500/10 text-accent-600 dark:text-accent-400'
                : 'text-ink-700 hover:bg-accent-500/10 hover:text-accent-500 dark:text-ink-200'
            "
          >
            {{ link.label[lang] }}
          </a>
        </li>
      </ul>
    </Transition>
  </header>
</template>
