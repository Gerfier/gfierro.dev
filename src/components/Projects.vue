<script setup>
import { computed, ref } from "vue";

const projects = [
  {
    title: { en: "Micro-frontend decoupling", es: "Desacoplamiento de micro-frontends" },
    company: { en: "Virtana", es: "Virtana" },
    description: {
      en: "Refactored core Zenoss product features into standalone, reusable micro-frontends to support a strategic platform partnership, and migrated several pages to a shared query-criteria structure to unify data flow across APIs.",
      es: "Refactoricé funciones clave del producto de Zenoss en micro-frontends independientes y reutilizables para apoyar una alianza estratégica de plataforma, y migré varias páginas a una estructura común de criterios de consulta para unificar el flujo de datos entre APIs.",
    },
    tags: [{ en: "Vue.js", es: "Vue.js" }, { en: "Micro-frontends", es: "Micro-frontends" }, { en: "GraphQL", es: "GraphQL" }],
    link: null,
    featured: true,
  },
  {
    title: { en: "Inventory query builder", es: "Constructor de consultas de inventario" },
    company: { en: "Zenoss", es: "Zenoss" },
    description: {
      en: "Designed and built a dynamic query builder for entity retrieval with query-saving, used as the primary inventory exploration tool across an IT monitoring cloud platform.",
      es: "Diseñé y construí un constructor de consultas dinámico para la búsqueda de entidades con guardado de consultas, usado como la herramienta principal de exploración de inventario en una plataforma cloud de monitoreo de IT.",
    },
    tags: [{ en: "Vue.js", es: "Vue.js" }, { en: "Vuetify", es: "Vuetify" }, { en: "GraphQL", es: "GraphQL" }],
    link: null,
  },
  {
    title: { en: "Events & dashboard visualizations", es: "Visualizaciones de eventos y dashboards" },
    company: { en: "Zenoss", es: "Zenoss" },
    description: {
      en: "Built an events page with graph and pie-chart visualizations for notifications, plus filterable dashboard tiles with gauge charts for event and metrics analysis.",
      es: "Construí una página de eventos con visualizaciones de gráficas y pie charts para notificaciones, además de tiles de dashboard filtrables con gauge charts para análisis de eventos y métricas.",
    },
    tags: [{ en: "Vue.js", es: "Vue.js" }, { en: "Data viz", es: "Visualización de datos" }, { en: "Vuetify", es: "Vuetify" }],
    link: null,
  },
  {
    title: { en: "Vue 2 → 3 platform migration", es: "Migración de plataforma de Vue 2 a 3" },
    company: { en: "Zenoss", es: "Zenoss" },
    description: {
      en: "Co-led the migration from Vue 2 to Vue 3 across the product, moving the build from Webpack to Vite and state management from Vuex to Pinia — improving build speed and shrinking bundle size.",
      es: "Co-lideré la migración de Vue 2 a Vue 3 en todo el producto, pasando el build de Webpack a Vite y el manejo de estado de Vuex a Pinia — mejorando la velocidad de build y reduciendo el tamaño del bundle.",
    },
    tags: [{ en: "Vue.js", es: "Vue.js" }, { en: "Vite", es: "Vite" }, { en: "Pinia", es: "Pinia" }],
    link: null,
  },
  {
    title: { en: "Camp counselor operations app", es: "App de operaciones para consejeros de campamento" },
    company: { en: "Personal project", es: "Proyecto personal" },
    description: {
      en: "Built a full-stack scheduling, points, messaging, and live map app for a youth summer camp's counselor and director staff, end to end. Realtime updates via Socket.io, an installable offline-friendly PWA, and push notifications for schedule changes — deployed as a single Docker/Fly.io service with a persistent volume.",
      es: "Construí de principio a fin una app full-stack de horarios, puntos, mensajería y mapa en vivo para el staff de consejeros y directores de un campamento de verano juvenil. Actualizaciones en tiempo real vía Socket.io, una PWA instalable que funciona offline, y notificaciones push para cambios de horario — desplegada como un solo servicio Docker/Fly.io con volumen persistente.",
    },
    tags: [{ en: "React", es: "React" }, { en: "Node.js", es: "Node.js" }, { en: "Socket.io", es: "Socket.io" }, { en: "PWA", es: "PWA" }],
    link: null,
  },
  {
    title: { en: "Booking site for a family resort", es: "Sitio de reservaciones para balneario familiar" },
    company: { en: "Los Filtros · Gibrando's Park", es: "Los Filtros · Gibrando's Park" },
    description: {
      en: "Built and shipped a static booking site for a family-run resort in San Francisco de Conchos, Chihuahua — cabin/camping pricing, a no-login admin panel for editing rates and events, and WhatsApp-based reservations that also create Google Calendar events automatically. No server, no hosting cost.",
      es: "Construí y publiqué un sitio de reservaciones estático para un balneario familiar en San Francisco de Conchos, Chihuahua — precios de cabañas y camping, un panel de administración sin contraseña para editar precios y eventos, y reservaciones por WhatsApp que también crean eventos en Google Calendar automáticamente. Sin servidor, sin costo de hosting.",
    },
    tags: [{ en: "JavaScript", es: "JavaScript" }, { en: "GitHub Pages", es: "GitHub Pages" }, { en: "Google Apps Script", es: "Google Apps Script" }],
    link: "https://github.com/Gerfier/los-filtros-gibrandos-park",
  },
  {
    title: { en: "Marketing site for a specialty coffee shop", es: "Sitio de marketing para cafetería de especialidad" },
    company: { en: "Nómada Café de Especialidad", es: "Nómada Café de Especialidad" },
    description: {
      en: "Designed and built a single-page marketing site for a specialty coffee shop in Ciudad Camargo, Chihuahua — real photos sourced from their Instagram, structured data for local SEO and Google Maps, scroll-reveal animations, and a fully responsive layout. Plain HTML/CSS/JS, no build step.",
      es: "Diseñé y construí un sitio de una sola página para una cafetería de especialidad en Ciudad Camargo, Chihuahua — fotos reales tomadas de su Instagram, datos estructurados para SEO local y Google Maps, animaciones al hacer scroll, y un diseño completamente responsivo. HTML/CSS/JS puro, sin build.",
    },
    tags: [{ en: "HTML/CSS", es: "HTML/CSS" }, { en: "JavaScript", es: "JavaScript" }, { en: "SEO", es: "SEO" }],
    link: "https://github.com/Gerfier/nomada-cafe",
  },
  {
    title: { en: "Cross-border money & tax companion", es: "Compañero financiero y fiscal transfronterizo" },
    company: { en: "Personal project", es: "Proyecto personal" },
    description: {
      en: "A React app for tracking finances as a contractor living between the US and Mexico — dual-currency budgeting, net worth, US/Mexico tax context, and residency day-counting. Vercel serverless functions keep the Anthropic API key server-side for statement imports and live market/FX snapshots; all data stays in the browser.",
      es: "Una app en React para llevar tus finanzas como contratista que vive entre EE.UU. y México — presupuesto en dos monedas, patrimonio neto, contexto fiscal de EE.UU./México, y conteo de días de residencia. Funciones serverless de Vercel mantienen la llave de la API de Anthropic en el servidor para importar estados de cuenta y obtener mercado/tipo de cambio en vivo; todos los datos se quedan en el navegador.",
    },
    tags: [{ en: "React", es: "React" }, { en: "Vite", es: "Vite" }, { en: "Vercel", es: "Vercel" }],
    link: "https://github.com/Gerfier/clearline",
  },
];

const allTags = computed(() => {
  const seen = new Map();
  for (const project of projects) {
    for (const tag of project.tags) {
      if (!seen.has(tag.en)) seen.set(tag.en, tag);
    }
  }
  return [{ en: "All", es: "Todos" }, ...seen.values()];
});
const activeTag = ref("All");

const filtered = computed(() =>
  activeTag.value === "All" ? projects : projects.filter((p) => p.tags.some((tag) => tag.en === activeTag.value))
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
        :key="tag.en"
        @click="activeTag = tag.en"
        type="button"
        class="rounded-full px-4 py-1.5 text-sm font-medium transition-colors"
        :class="
          activeTag === tag.en
            ? 'bg-ink-950 text-white dark:bg-white dark:text-ink-950'
            : 'border border-ink-200 text-ink-600 hover:border-accent-500 hover:text-accent-600 dark:border-ink-700 dark:text-ink-300 dark:hover:border-accent-400 dark:hover:text-accent-400'
        "
      >
        <span data-i18n-lang="en" class="i18n-inline">{{ tag.en }}</span>
        <span data-i18n-lang="es" class="i18n-inline">{{ tag.es }}</span>
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
        :key="project.title.en"
        @pointermove="onCardMove"
        @pointerleave="onCardLeave"
        class="gradient-border group rounded-2xl border border-ink-200 bg-ink-100/30 p-6 transition-[transform,border-color] duration-150 ease-out will-change-transform hover:border-accent-500/50 dark:border-ink-800 dark:bg-ink-900/30"
        :class="project.featured ? 'sm:col-span-2' : ''"
      >
        <p class="font-mono text-xs text-ink-500 dark:text-ink-400">
          <span data-i18n-lang="en" class="i18n-inline">{{ project.company.en }}</span>
          <span data-i18n-lang="es" class="i18n-inline">{{ project.company.es }}</span>
        </p>
        <h3 class="mt-1 text-lg font-bold text-ink-950 dark:text-white">
          <span data-i18n-lang="en" class="i18n-inline">{{ project.title.en }}</span>
          <span data-i18n-lang="es" class="i18n-inline">{{ project.title.es }}</span>
        </h3>
        <p class="mt-3 text-sm leading-relaxed text-ink-600 dark:text-ink-300">
          <span data-i18n-lang="en">{{ project.description.en }}</span>
          <span data-i18n-lang="es">{{ project.description.es }}</span>
        </p>
        <div class="mt-4 flex flex-wrap gap-2">
          <span
            v-for="tag in project.tags"
            :key="tag.en"
            class="rounded-full bg-ink-100 px-2.5 py-1 text-xs font-medium text-ink-600 dark:bg-ink-800 dark:text-ink-300"
          >
            <span data-i18n-lang="en" class="i18n-inline">{{ tag.en }}</span>
            <span data-i18n-lang="es" class="i18n-inline">{{ tag.es }}</span>
          </span>
        </div>
        <a
          v-if="project.link"
          :href="project.link"
          target="_blank"
          rel="noreferrer"
          class="mt-4 inline-flex items-center gap-1.5 text-sm font-semibold text-accent-600 transition-colors hover:text-accent-500 dark:text-accent-400"
        >
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="h-4 w-4">
            <path
              d="M12 2C6.48 2 2 6.58 2 12.25c0 4.5 2.87 8.32 6.84 9.67.5.1.68-.22.68-.49 0-.24-.01-1.05-.01-1.9-2.78.62-3.37-1.21-3.37-1.21-.45-1.18-1.11-1.5-1.11-1.5-.9-.63.07-.62.07-.62 1 .07 1.53 1.05 1.53 1.05.89 1.56 2.34 1.11 2.91.85.09-.65.34-1.11.62-1.36-2.22-.26-4.56-1.14-4.56-5.06 0-1.12.39-2.03 1.03-2.75-.1-.26-.45-1.32.1-2.75 0 0 .84-.27 2.76 1.05a9.36 9.36 0 0 1 5.02 0c1.92-1.32 2.76-1.05 2.76-1.05.55 1.43.2 2.49.1 2.75.64.72 1.03 1.63 1.03 2.75 0 3.93-2.35 4.8-4.58 5.05.36.32.68.94.68 1.9 0 1.37-.01 2.48-.01 2.81 0 .27.18.6.69.49A10.02 10.02 0 0 0 22 12.25C22 6.58 17.52 2 12 2z"
            />
          </svg>
          <span data-i18n-lang="en" class="i18n-inline">View on GitHub</span>
          <span data-i18n-lang="es" class="i18n-inline">Ver en GitHub</span>
        </a>
      </article>
    </TransitionGroup>

    <p class="mt-8 text-sm text-ink-500 dark:text-ink-400">
      <span data-i18n-lang="en">
        The Virtana/Zenoss work summarizes professional roles under NDA, so no code links there. The
        camp app is a personal project — its source stays private out of respect for the organization's
        branding and facility details, but I'm happy to walk through the code in an interview.
      </span>
      <span data-i18n-lang="es">
        El trabajo de Virtana/Zenoss resume roles profesionales bajo NDA, por lo que no hay enlaces de
        código ahí. La app del campamento es un proyecto personal — su código se mantiene privado por
        respeto a la marca y los detalles de las instalaciones de la organización, pero con gusto puedo
        revisar el código en una entrevista.
      </span>
    </p>
  </div>
</template>
