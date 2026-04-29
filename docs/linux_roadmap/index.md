# Linux Roadmap

Welcome to the Linux Roadmap section of my portfolio. Here you will find comprehensive guides and in-depth analyses covering a wide range of Linux topics — from setting up applications to understanding the ecosystem at a deeper level.

This section is divided into two focused categories. **Application Setups** provides step-by-step configuration guides for tools and environments on Linux. **Technical Articles** offers thoughtful comparisons, analyses, and explorations of distributions, desktop environments, and workflows. Feel free to dive into whichever suits your current needs.

!!! note "Continuously Updated"
    This section is actively maintained and regularly expanded with new guides and articles. If any content feels outdated or you have suggestions for new topics, don't hesitate to reach out!

---

<div class="roadmap-cards">

  <a href="app_setups_in_linux/" class="roadmap-card">
    <div class="roadmap-card-icon">⚙️</div>
    <div class="roadmap-card-body">
      <h2 class="roadmap-card-title">Application Setups in Linux</h2>
      <p class="roadmap-card-desc">
        Step-by-step guides for installing and configuring real-world applications on Linux — from SQL engines and Python environments to code editors, shell tools, and multimedia utilities.
      </p>
      <div class="roadmap-card-meta">
        <span class="roadmap-meta-tag">Shell</span>
        <span class="roadmap-meta-tag">Docker</span>
        <span class="roadmap-meta-tag">Python</span>
        <span class="roadmap-meta-tag">Neovim</span>
        <span class="roadmap-meta-tag">FFmpeg</span>
      </div>
      <span class="roadmap-card-link">Explore Guides →</span>
    </div>
  </a>

  <a href="linux_articles/" class="roadmap-card">
    <div class="roadmap-card-icon">📰</div>
    <div class="roadmap-card-body">
      <h2 class="roadmap-card-title">Technical Articles about Linux</h2>
      <p class="roadmap-card-desc">
        In-depth technical articles exploring Linux distributions, desktop environments, system internals, and best practices — designed to help you make informed decisions and deepen your Linux knowledge.
      </p>
      <div class="roadmap-card-meta">
        <span class="roadmap-meta-tag">Distros</span>
        <span class="roadmap-meta-tag">Desktop Environments</span>
        <span class="roadmap-meta-tag">System Tools</span>
        <span class="roadmap-meta-tag">Workflows</span>
      </div>
      <span class="roadmap-card-link">Read Articles →</span>
    </div>
  </a>

</div>

<style>
/* ── Grid ── */
.roadmap-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-top: 2.5rem;
}

@media (max-width: 768px) {
  .roadmap-cards {
    grid-template-columns: 1fr;
  }
}

/* ── Card ── */
.roadmap-card {
  display: flex;
  flex-direction: column;
  text-decoration: none !important;
  border: 1px solid rgba(83, 120, 210, 0.2);
  border-top: 3px solid rgba(83, 120, 210, 0.45);
  border-radius: 10px;
  padding: 2rem 1.8rem 1.6rem;
  background-color: rgba(83, 120, 210, 0.04);
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease, background-color 0.25s ease;
  cursor: pointer;
}

.roadmap-card:hover {
  transform: translateY(-5px);
  background-color: rgba(83, 120, 210, 0.09);
  border-color: rgba(83, 120, 210, 0.6);
  border-top-color: var(--md-primary-fg-color);
  box-shadow: 0 8px 32px rgba(83, 120, 210, 0.18);
  text-decoration: none !important;
}

/* ── Icon ── */
.roadmap-card-icon {
  font-size: 2.2rem;
  margin-bottom: 1.1rem;
  line-height: 1;
}

/* ── Body ── */
.roadmap-card-body {
  display: flex;
  flex-direction: column;
  flex: 1;
}

/* ── Title ── */
.roadmap-card-title {
  font-size: 1.1rem !important;
  font-weight: 700 !important;
  color: rgba(255, 255, 255, 0.95) !important;
  margin: 0 0 0.75rem 0 !important;
  padding: 0 !important;
  border: none !important;
  line-height: 1.35 !important;
}

.roadmap-card-title::before {
  display: none !important;
}

/* ── Description ── */
.roadmap-card-desc {
  font-size: 0.82rem !important;
  color: rgba(255, 255, 255, 0.55) !important;
  line-height: 1.75 !important;
  margin: 0 0 1.2rem 0 !important;
  flex: 1;
}

/* ── Meta Tags ── */
.roadmap-card-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-bottom: 1.3rem;
}

.roadmap-meta-tag {
  font-size: 10px;
  font-family: "Fira Code", monospace !important;
  font-weight: 500;
  letter-spacing: 0.4px;
  padding: 2px 9px;
  border-radius: 999px;
  background-color: rgba(83, 120, 210, 0.15);
  color: rgba(150, 180, 255, 0.85);
  border: 1px solid rgba(83, 120, 210, 0.25);
}

/* ── CTA Link ── */
.roadmap-card-link {
  display: inline-block;
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--md-primary-fg-color) !important;
  letter-spacing: 0.2px;
  margin-top: auto;
  transition: opacity 0.2s ease;
}

.roadmap-card:hover .roadmap-card-link {
  opacity: 0.75;
}

/* ── Light Mode ── */
[data-md-color-scheme="default"] .roadmap-card {
  background-color: rgba(63, 100, 200, 0.03);
  border-color: rgba(63, 100, 200, 0.15);
  border-top-color: rgba(63, 100, 200, 0.4);
}

[data-md-color-scheme="default"] .roadmap-card:hover {
  background-color: rgba(63, 100, 200, 0.07);
  border-color: rgba(63, 100, 200, 0.45);
  border-top-color: var(--md-primary-fg-color);
  box-shadow: 0 8px 32px rgba(63, 100, 200, 0.1);
}

[data-md-color-scheme="default"] .roadmap-card-title {
  color: rgba(0, 0, 0, 0.88) !important;
}

[data-md-color-scheme="default"] .roadmap-card-desc {
  color: rgba(0, 0, 0, 0.55) !important;
}

[data-md-color-scheme="default"] .roadmap-meta-tag {
  background-color: rgba(63, 100, 200, 0.08);
  color: rgba(40, 80, 180, 0.9);
  border-color: rgba(63, 100, 200, 0.2);
}
</style>