<template>
  <div class="raster-container">
    <!-- Header Section -->
    <div class="raster-section-header">
      <span class="raster-index">[ 00 ]</span>
      <h1 class="raster-h1">Technical Specifications & Manual</h1>
      <div class="raster-rule"></div>
      <span class="raster-meta">RASTER CSS // CORE CODEX</span>
    </div>

    <div class="raster-grid">
      <!-- Main Overview -->
      <div class="col-8">
        <div class="raster-card mb-6">
          <div class="raster-card-header">
            <span class="raster-label">System Architecture</span>
            <span class="raster-tag raster-tag-accent raster-mono">[ ZERO-AI-SLOP ]</span>
          </div>
          <p class="raster-lead mb-4">
            Online Photo Filter is an open-source, client-side digital darkroom engineered strictly around the
            <strong>Raster CSS (Swiss International Typographic Style)</strong> design system.
          </p>
          <p class="raster-body mb-4">
            Unlike modern homogenized AI-generated interfaces cluttered with glowing indigo drop-shadows, 9999px pills,
            and muddy background gradients, this application enforces strict 0px geometric boundaries, 1px architectural
            hairline dividers, tabular typography, and surgical Swiss Vermillion signal indicators.
          </p>
          <div class="raster-callout raster-callout-accent mt-4">
            <span class="raster-label raster-label-accent">Privacy Specification</span>
            <p class="raster-small">
              All pixel operations, CSS filtering, light leaks, and canvas rasterizations are performed 100% locally
              within your browser. No photo data or telemetry ever leaves your machine.
            </p>
          </div>
        </div>

        <!-- Keyboard Controls Ledger -->
        <div class="raster-card">
          <div class="raster-card-header">
            <span class="raster-label">Keyboard Directives // Quick Controls</span>
            <span class="raster-mono text-muted">[ INPUT MAP ]</span>
          </div>

          <div class="raster-table-container">
            <table class="raster-table">
              <thead>
                <tr>
                  <th>Keystroke</th>
                  <th>Action</th>
                  <th>Scope</th>
                  <th class="cell-num">Status</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code class="raster-mono text-accent">Z</code></td>
                  <td>Cycle previous filter preset</td>
                  <td>Active Studio</td>
                  <td class="cell-num raster-mono">[ ACTIVE ]</td>
                </tr>
                <tr>
                  <td><code class="raster-mono text-accent">X</code></td>
                  <td>Cycle next filter preset</td>
                  <td>Active Studio</td>
                  <td class="cell-num raster-mono">[ ACTIVE ]</td>
                </tr>
                <tr>
                  <td><code class="raster-mono text-accent">&larr;</code></td>
                  <td>Step previous blend mode</td>
                  <td>Light Leak FX</td>
                  <td class="cell-num raster-mono">[ ACTIVE ]</td>
                </tr>
                <tr>
                  <td><code class="raster-mono text-accent">&rarr;</code></td>
                  <td>Step next blend mode</td>
                  <td>Light Leak FX</td>
                  <td class="cell-num raster-mono">[ ACTIVE ]</td>
                </tr>
                <tr>
                  <td><code class="raster-mono text-accent">ESC</code></td>
                  <td>Reset canvas to Clean Base</td>
                  <td>Global Canvas</td>
                  <td class="cell-num raster-mono">[ ACTIVE ]</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- Sidebar Specs -->
      <div class="col-4">
        <div class="raster-card mb-6">
          <div class="raster-card-header">
            <span class="raster-label">Core Engine Stats</span>
            <span class="raster-mono text-muted">[ 01 ]</span>
          </div>

          <div class="raster-stat mb-4">
            <span class="raster-stat-label">Total Presets</span>
            <span class="raster-stat-value">48+</span>
            <div class="raster-stat-sub">
              <span class="raster-delta raster-delta-positive">100% CSS</span>
              <span>Zero external runtime overhead</span>
            </div>
          </div>

          <hr class="raster-hr" />

          <div class="raster-stat mb-4">
            <span class="raster-stat-label">Corner Geometry</span>
            <span class="raster-stat-value">0 PX</span>
            <div class="raster-stat-sub">
              <span>Strict 90-degree architectural discipline</span>
            </div>
          </div>

          <hr class="raster-hr" />

          <div class="raster-stat">
            <span class="raster-stat-label">Color Accent</span>
            <span class="raster-stat-value text-accent">#FF3B14</span>
            <div class="raster-stat-sub">
              <span>Swiss Vermillion Signal Indicator</span>
            </div>
          </div>
        </div>

        <div class="raster-card raster-card-subtle">
          <div class="raster-label mb-2">Documentation & Repository</div>
          <p class="raster-small mb-3">
            Raster CSS design guidelines and source code available on GitHub.
          </p>
          <a
            href="https://github.com/arron21/raster-css"
            target="_blank"
            rel="noopener noreferrer"
            class="raster-btn raster-btn-outline w-full text-center"
          >
            [ GITHUB / RASTER-CSS ] &rarr;
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
code {
  font-family: var(--raster-font-mono);
  font-weight: 700;
  padding: 2px 6px;
  background-color: var(--raster-surface-subtle);
  border: 1px solid var(--raster-border);
}
</style>
