# Technical Articles about Linux

In this section, I have compiled a collection of technical articles that delve into various aspects of Linux — including system administration, command-line tools, scripting, and performance optimization. Each article is meticulously researched to offer clear explanations, step-by-step guides, and real-world examples to help you enhance your Linux skills and make the most out of your experience. Whether you're just getting started or seeking advanced techniques, these articles will serve as valuable resources throughout your Linux journey.

---

<div class="article-list">

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Opinion · Philosophy</span>
    <h2 class="article-title">Why Should / Shouldn't You Use Linux?</h2>
    <p class="article-desc">An exploration of the advantages and trade-offs of using Linux as a daily operating system — covering its open-source nature, security model, customization depth, and performance capabilities, along with an honest look at where it may not be the right fit.</p>
    <a href="why_should_you_use_linux.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Distros · Beginner Guide</span>
    <h2 class="article-title">Which Linux Distro Should You Use?</h2>
    <p class="article-desc">An in-depth analysis of popular Linux distributions — their unique philosophies, package ecosystems, stability models, and target audiences — with practical recommendations based on your background and goals.</p>
    <a href="which_linux_distro_should_you_use.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Desktop Environments · Comparison</span>
    <h2 class="article-title">Best Desktop Environments for Linux</h2>
    <p class="article-desc">A side-by-side comparison of the most popular Linux desktop environments — GNOME, KDE Plasma, XFCE, Cinnamon, and more — evaluating their features, resource usage, and suitability for different types of users and workflows.</p>
    <a href="best_desktop_environments_for_linux.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Package Managers · Deep Dive</span>
    <h2 class="article-title">APT vs DNF vs Pacman vs Zypper vs Portage vs XBPS</h2>
    <p class="article-desc">A comprehensive comparison of the major Linux package managers — their dependency resolution strategies, speed, flexibility, and ecosystem fit — with clear recommendations to help you understand what your distro's tooling is actually doing under the hood.</p>
    <a href="apt_vs_dnf_vs_pacman_vs_zypper_vs_portage_vs_xbps.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Distros · Personal Pick</span>
    <h2 class="article-title">Why I Use CachyOS Linux</h2>
    <p class="article-desc">A personal deep-dive into the CachyOS Linux distribution — what sets it apart from mainstream Arch-based distros, its performance-tuned kernel, BORE scheduler, and why it has become my daily driver of choice.</p>
    <a href="why_i_use_cachyos_linux_distro.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Shell · Comparison</span>
    <h2 class="article-title">Which Linux Shell Should You Use?</h2>
    <p class="article-desc">An analysis of popular Linux shells — Bash, Zsh, Fish, Nushell, and others — comparing their scripting power, interactive features, plugin ecosystems, and overall user experience, with recommendations tailored to different use cases.</p>
    <a href="which_linux_shell_should_you_use.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Terminal Emulators · Benchmark</span>
    <h2 class="article-title">Konsole vs Terminal vs Alacritty vs Kitty vs WezTerm vs Hyper</h2>
    <p class="article-desc">A comprehensive head-to-head comparison of the most widely used Linux terminal emulators — evaluating rendering performance, GPU acceleration, configurability, font rendering, and day-to-day usability to help you find the one that fits your workflow.</p>
    <a href="terminal_emulator_comparison.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">DE vs WM · Concepts</span>
    <h2 class="article-title">Desktop Environment or Window Manager?</h2>
    <p class="article-desc">An exploration of the fundamental differences between desktop environments and standalone window managers in Linux — their feature sets, resource footprints, customization potential, and which type of user each approach is best suited for.</p>
    <a href="desktop_environment_or_window_manager.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Window Managers · Comparison</span>
    <h2 class="article-title">Niri vs Hyprland</h2>
    <p class="article-desc">A focused comparison of two modern Wayland-native window managers — Niri's scrollable tiling model and Hyprland's dynamic compositing approach — examining their configuration, animation systems, stability, and daily usability.</p>
    <a href="niri_vs_hyprland.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

<div class="article-card">
  <div class="article-card-inner">
    <span class="article-tag">Wayland · Display Server</span>
    <h2 class="article-title">What is Wayland? Differences from X11</h2>
    <p class="article-desc">An introduction to the Wayland display server protocol — how it fundamentally rethinks the Linux graphics stack compared to the legacy X11 system, its security advantages, current ecosystem maturity, and what the transition means for everyday Linux users.</p>
    <a href="what_is_wayland_differences_from_x11.md" class="article-read-link">Read Full Article →</a>
  </div>
</div>

</div>

<style>
/* ── Article List Container ── */
.article-list {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin-top: 2rem;
}

/* ── Article Card ── */
.article-card {
  border: 1px solid rgba(83, 120, 210, 0.18);
  border-left: 3px solid rgba(83, 120, 210, 0.35);
  border-radius: 6px;
  transition: background-color 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
  margin-bottom: 1rem;
}

.article-card:hover {
  background-color: rgba(83, 120, 210, 0.06);
  border-color: rgba(83, 120, 210, 0.65);
  border-left-color: var(--md-primary-fg-color);
  box-shadow: 0 2px 16px rgba(83, 120, 210, 0.12);
}

.article-card-inner {
  padding: 1.4rem 1.2rem 1.4rem 1.4rem;
}

/* ── Tag ── */
.article-tag {
  display: inline-block;
  font-size: 10.5px;
  font-weight: 600;
  letter-spacing: 0.6px;
  text-transform: uppercase;
  color: var(--md-primary-fg-color);
  margin-bottom: 0.45rem;
  font-family: "Fira Code", monospace !important;
}

/* ── Title ── */
.article-title {
  font-size: 1.05rem !important;
  font-weight: 600 !important;
  margin: 0 0 0.5rem 0 !important;
  padding: 0 !important;
  color: rgba(255, 255, 255, 0.92) !important;
  line-height: 1.4 !important;
  border: none !important;
}

.article-title::before {
  display: none !important;
}

/* ── Description ── */
.article-desc {
  font-size: 0.82rem !important;
  color: rgba(255, 255, 255, 0.55) !important;
  margin: 0 0 0.85rem 0 !important;
  line-height: 1.7 !important;
  max-width: 72ch;
}

/* ── Read Link ── */
.article-read-link {
  display: inline-block;
  font-size: 0.78rem;
  font-weight: 600;
  color: var(--md-primary-fg-color) !important;
  text-decoration: none !important;
  letter-spacing: 0.2px;
  transition: opacity 0.2s ease;
}

.article-read-link:hover {
  opacity: 0.75;
  text-decoration: underline !important;
}

/* ── Light Mode ── */
[data-md-color-scheme="default"] .article-card {
  border: 1px solid rgba(63, 100, 200, 0.15);
  border-left: 3px solid rgba(63, 100, 200, 0.3);
}

[data-md-color-scheme="default"] .article-card:hover {
  background-color: rgba(63, 100, 200, 0.04);
  border-color: rgba(63, 100, 200, 0.5);
  border-left-color: var(--md-primary-fg-color);
  box-shadow: 0 2px 16px rgba(63, 100, 200, 0.08);
}

[data-md-color-scheme="default"] .article-title {
  color: rgba(0, 0, 0, 0.88) !important;
}

[data-md-color-scheme="default"] .article-desc {
  color: rgba(0, 0, 0, 0.55) !important;
}
</style>