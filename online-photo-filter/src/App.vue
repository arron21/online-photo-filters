<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RouterLink, RouterView, useRoute } from 'vue-router'

const route = useRoute()
const currentTheme = ref('dark')

const toggleTheme = () => {
  currentTheme.value = currentTheme.value === 'dark' ? 'light' : 'dark'
  document.documentElement.setAttribute('data-theme', currentTheme.value)
  try {
    localStorage.setItem('raster-theme', currentTheme.value)
  } catch (e) {
    // ignore
  }
}

onMounted(() => {
  try {
    const saved = localStorage.getItem('raster-theme')
    if (saved === 'light' || saved === 'dark') {
      currentTheme.value = saved
    }
  } catch (e) {
    // ignore
  }
  document.documentElement.setAttribute('data-theme', currentTheme.value)
})
</script>

<template>
  <div class="raster-app">
    <!-- Architectural Swiss Masthead -->
    <header class="raster-masthead">
      <div class="raster-container raster-masthead-inner">
        <div class="raster-flex raster-items-center gap-4">
          <RouterLink to="/" class="raster-brand">
            <span class="raster-brand-dot"></span>
            <span>Online Photo Filter</span>
          </RouterLink>
          <span class="raster-tag raster-mono d-none-sm">[ v2.0 // RASTER ]</span>
        </div>

        <nav class="raster-flex raster-items-center gap-6">
          <ul class="raster-nav-links">
            <li>
              <RouterLink
                to="/"
                class="raster-nav-link"
                :class="{ 'is-active': route.path === '/' }"
              >
                // STUDIO
              </RouterLink>
            </li>
            <li>
              <RouterLink
                to="/about"
                class="raster-nav-link"
                :class="{ 'is-active': route.path === '/about' }"
              >
                // SPECS
              </RouterLink>
            </li>
          </ul>

          <div class="raster-flex raster-items-center gap-2">
            <button
              @click="toggleTheme"
              class="raster-btn raster-btn-ghost raster-btn-sm raster-mono"
              :title="'Switch to ' + (currentTheme === 'dark' ? 'light paper' : 'dark carbon') + ' mode'"
            >
              [ THEME: {{ currentTheme.toUpperCase() }} ]
            </button>
          </div>
        </nav>
      </div>
    </header>

    <!-- Main Workspace Viewport -->
    <main class="raster-main">
      <RouterView />
    </main>

    <!-- Architectural Swiss Ledger Footer -->
    <footer class="raster-footer">
      <div class="raster-container">
        <div class="raster-grid">
          <div class="col-4">
            <div class="raster-label mb-1">Application // System</div>
            <div class="raster-mono text-primary" style="font-weight: 600;">Online Photo Filter dot com</div>
            <div class="raster-micro mt-1 text-muted">
              Swiss International Typographic Design System · 0px Sharp Geometry
            </div>
          </div>

          <div class="col-4">
            <div class="raster-label mb-1">Architecture // Tokens</div>
            <div class="raster-mono text-primary">Engineered with Raster CSS</div>
            <div class="raster-micro mt-1 text-muted">
              Tabular numerals · Hairline borders · High-signal Vermillion
            </div>
          </div>

          <div class="col-4 text-right">
            <div class="raster-label mb-1">Author // Licensing</div>
            <div class="raster-mono text-primary">Arron McCrory</div>
            <div class="raster-micro mt-1 text-muted">
              [ MIT LICENSE // 2026 ]
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.raster-app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: var(--raster-canvas);
  color: var(--raster-fg-primary);
}

.raster-main {
  flex: 1;
  padding-top: var(--raster-space-6);
  padding-bottom: var(--raster-space-12);
}

.raster-footer {
  border-top: 1px solid var(--raster-border);
  background-color: var(--raster-surface);
  padding-top: var(--raster-space-6);
  padding-bottom: var(--raster-space-8);
  margin-top: auto;
}

@media (max-width: 640px) {
  .d-none-sm {
    display: none;
  }
}
</style>
