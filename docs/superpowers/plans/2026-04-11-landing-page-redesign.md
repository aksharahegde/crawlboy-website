# Crawlboy Landing Page Redesign - Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Merge the best elements from three existing design alternatives into a single, polished landing page with accurate crawlboy content, subtle animations, an interactive terminal demo, and proper light/dark theme support.

**Architecture:** Single-file HTML page using Tailwind CDN with the existing Material Design 3 color system from DESIGN.md. Content is sourced exclusively from the crawlboy README — no fabricated features. The page follows a linear narrative flow: Hero (what it does) → Live Demo (see it work) → Features (why use it) → Quick Start (how to install) → CLI Reference (flags) → CTA + Footer.

**Tech Stack:** HTML, Tailwind CSS (CDN), vanilla JavaScript (scroll animations, terminal typing, tab switching, copy-to-clipboard, theme toggle), Google Fonts (Space Grotesk, Inter, JetBrains Mono), Material Symbols Outlined.

---

## Content Audit — What's Real vs. Fabricated

Before building, here's the ground truth from crawlboy's README. **Every piece of text on the landing page must come from this list.**

### Real features:
- Sitemap discovery from robots.txt or common paths
- Nested sitemap index support (`<sitemapindex>`)
- Markdown output — one `.md` per page, URL structure mirrored
- HTML output — optional raw HTML under `html/`
- Image download — content-addressed, deduplicated, paths rewritten
- Interactive CLI wizard (questionary + Rich)
- Docker support (headless out of the box)

### Real CLI flags:
| Flag | Description | Default |
|---|---|---|
| `--sitemap-url` | Direct sitemap URL | — |
| `--site-url` | Site root for auto-discovery | — |
| `--out-dir` | Output directory | — |
| `--delay` | Seconds to wait after each page | 0 |
| `--page-timeout-ms` | Navigation timeout in ms | 60000 |
| `--max-urls` | Cap number of URLs | unlimited |
| `--save-html` | Write raw HTML under `html/` | off |
| `--download-images` | Save images under `media/` | off |
| `--no-headless` | Show the browser window | off |
| `--fail-fast` | Stop on first crawl error | off |
| `--include-offsite-urls` | Crawl hosts outside site origin | off |
| `-i`, `--interactive` | Launch guided wizard | off |

### Real install command:
```
pip install crawlboy
crawl4ai-setup
```

### Real usage examples:
```
crawlboy --sitemap-url 'https://example.com/sitemap.xml' --out-dir ./out
crawlboy --site-url 'https://www.example.com' --out-dir ./out
crawlboy --interactive
```

### Real output structure:
```
/blog/articles/basic-git-commands/
→ {out-dir}/md/blog/articles/basic-git-commands.md
```
Site root `/` maps to `{out-dir}/md/index.md`. Failures logged to `{out-dir}/errors.jsonl`.

### Things that DO NOT exist (found in current designs — must be removed):
- `Crawler` Python class / `bot.ignite()` API
- `--recursive`, `--depth`, `--threads`, `--fast`, `--output`, `--quiet` flags
- Proxy rotation, auto-export to S3, custom hooks
- Regex filtering, JSON serialization, Pydantic support
- "Status: Stable 2.4.0", "Latency: 14ms"
- `concurrency` parameter
- Any "Kinetic Terminal Core" branding (design system name, not product name)

---

## Design Decisions — What to Take From Each Design

### From index.html (current):
- Glassmorphic nav with backdrop-blur
- Sidebar "On this page" navigation (desktop only)
- Terminal window component styling (header bar with label + dots)
- Installation tabs (pip / source / docker)
- Tailwind config with full M3 color tokens

### From alt.html:
- Hero layout: text left, terminal right (2-column)
- Inline install command with copy button in hero
- "Why Crawlboy?" section with 3-column feature cards using tonal shifts
- Asymmetric visual showcase section
- Final dark CTA section with install command
- "Read Documentation" link with arrow

### From alt_2.html:
- Bold oversized hero typography (text-8xl, tracking-tighter, leading-[0.9])
- Feature cards with inline code snippets showing real commands
- Bento grid for secondary features (2-col + 1-col + 1-col)
- Live session terminal preview with realistic output
- Icon hover effect (bg changes to primary-container on hover)
- Monospace label above hero headline ("Core Architecture" style)

### New additions (not in any design):
- **Interactive terminal demo** — typing animation that cycles through real commands
- **Theme toggle button** in nav (sun/moon icon, toggles `dark` class)
- **Scroll-triggered animations** via IntersectionObserver (fade-in-up for sections)
- **Copy-to-clipboard** on install commands with visual feedback
- **Output structure visualization** — animated tree showing URL → file mapping
- **Smooth scroll** for anchor links

---

## Page Structure (Top to Bottom)

```
1. NAV — glassmorphic, logo left, links center, theme toggle + Install CTA right
2. HERO — oversized headline + subtitle left, animated terminal right
3. FEATURES — "Why Crawlboy?" 3-column grid (Sitemap Discovery, Markdown Output, Image Download)
4. LIVE DEMO — full-width dark terminal showing real crawl session with typing animation
5. HOW IT WORKS — asymmetric section: output tree visualization left, explanation right
6. QUICK START — tabbed install (pip/source/docker) with copy buttons
7. CLI REFERENCE — striped table of real flags
8. CTA — dark banner with install command + Get Started button
9. FOOTER — logo, links (GitHub, Docs, PyPI, License), "Built by Akshara Hegde"
```

---

## File Structure

Only one file is created/modified:

- **Modify:** `index.html` — complete rewrite with merged design

The alt files (`alt.html`, `alt_2.html`) are left untouched as reference.

---

## Task 1: Scaffold — HTML structure, Tailwind config, nav, and theme toggle

**Files:**
- Modify: `index.html` (complete rewrite)

- [ ] **Step 1: Write the full HTML scaffold with Tailwind config and dark mode**

Replace the entire contents of `index.html` with the scaffold below. This sets up the Tailwind config (reusing the M3 color tokens from the current design), font imports, base styles, the glassmorphic nav with theme toggle, and empty section containers.

```html
<!doctype html>
<html class="light" lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Crawlboy — Sequentially crawl sitemaps and convert web pages to markdown with Crawl4AI." />
    <title>Crawlboy — Sitemap to Markdown</title>

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@300;500;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
    <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght,FILL@100..700,0..1&display=swap" rel="stylesheet" />

    <script src="https://cdn.tailwindcss.com?plugins=forms,container-queries"></script>
    <script>
        tailwind.config = {
            darkMode: "class",
            theme: {
                extend: {
                    colors: {
                        /* --- Existing M3 tokens from DESIGN.md (keep all) --- */
                        "surface-tint": "#006e16",
                        "on-surface": "#1c1b1b",
                        "inverse-surface": "#313030",
                        "surface-container-high": "#ebe7e7",
                        "on-surface-variant": "#3b4b37",
                        primary: "#006e16",
                        surface: "#fcf9f8",
                        "primary-container": "#00ff41",
                        "surface-container-low": "#f6f3f2",
                        "on-primary": "#ffffff",
                        "surface-container-lowest": "#ffffff",
                        secondary: "#5f5e5e",
                        "secondary-container": "#e5e2e1",
                        "surface-container-highest": "#e5e2e1",
                        "on-primary-fixed": "#002203",
                        "inverse-on-surface": "#f3f0ef",
                        "on-primary-container": "#007117",
                        "surface-variant": "#e5e2e1",
                        outline: "#6b7c65",
                        "surface-container": "#f0edec",
                        "inverse-primary": "#00e639",
                        "primary-fixed": "#72ff70",
                        "primary-fixed-dim": "#00e639",
                        "outline-variant": "#b9ccb2",
                        "on-secondary-container": "#656464",
                    },
                    fontFamily: {
                        headline: ["Space Grotesk", "sans-serif"],
                        body: ["Inter", "sans-serif"],
                        mono: ["JetBrains Mono", "monospace"],
                    },
                },
            },
        };
    </script>
    <style>
        body { font-family: "Inter", sans-serif; }
        .font-headline { font-family: "Space Grotesk", sans-serif; }
        .font-mono { font-family: "JetBrains Mono", monospace; }
        .material-symbols-outlined {
            font-variation-settings: "FILL" 0, "wght" 200, "GRAD" 0, "opsz" 24;
        }
        .terminal-glow {
            background: linear-gradient(135deg, #006e16 0%, #00e639 100%);
        }

        /* Scroll-triggered animation base */
        .reveal { opacity: 0; transform: translateY(24px); transition: opacity 0.7s cubic-bezier(0.16, 1, 0.3, 1), transform 0.7s cubic-bezier(0.16, 1, 0.3, 1); }
        .reveal.visible { opacity: 1; transform: translateY(0); }
        .reveal-delay-1 { transition-delay: 0.1s; }
        .reveal-delay-2 { transition-delay: 0.2s; }
        .reveal-delay-3 { transition-delay: 0.3s; }

        /* Terminal typing cursor */
        @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
        .cursor-blink { animation: blink 1s step-end infinite; }

        /* Dark mode overrides */
        .dark body { background-color: #1c1b1b; color: #f3f0ef; }
    </style>
</head>
<body class="bg-surface text-on-surface selection:bg-primary-container selection:text-on-primary-container">

    <!-- NAV -->
    <nav class="fixed top-0 w-full z-50 bg-[#fcf9f8]/80 dark:bg-[#313030]/80 backdrop-blur-md transition-colors">
        <div class="flex justify-between items-center w-full px-8 py-4 max-w-7xl mx-auto">
            <div class="flex items-center gap-8">
                <span class="text-xl font-bold tracking-tighter text-[#313030] dark:text-[#fcf9f8] font-headline">CRAWLBOY</span>
                <div class="hidden md:flex items-center gap-6">
                    <a href="#features" class="text-[#313030] dark:text-[#fcf9f8] opacity-70 hover:opacity-100 transition-opacity">Features</a>
                    <a href="#demo" class="text-[#313030] dark:text-[#fcf9f8] opacity-70 hover:opacity-100 transition-opacity">Demo</a>
                    <a href="#install" class="text-[#313030] dark:text-[#fcf9f8] opacity-70 hover:opacity-100 transition-opacity">Install</a>
                    <a href="#cli" class="text-[#313030] dark:text-[#fcf9f8] opacity-70 hover:opacity-100 transition-opacity">CLI</a>
                </div>
            </div>
            <div class="flex items-center gap-4">
                <button id="theme-toggle" class="material-symbols-outlined text-[#313030] dark:text-[#fcf9f8] opacity-70 hover:opacity-100 transition-opacity cursor-pointer" aria-label="Toggle theme">light_mode</button>
                <a href="#install" class="terminal-glow text-on-primary px-5 py-2 font-headline font-bold text-sm active:scale-95 duration-150 inline-block">Install</a>
            </div>
        </div>
    </nav>

    <main class="pt-32 pb-24">
        <!-- Sections will be added in subsequent tasks -->
    </main>

    <!-- FOOTER -->
    <footer class="w-full py-12 bg-surface-container-low dark:bg-[#313030] transition-colors">
        <div class="flex flex-col md:flex-row justify-between items-center px-8 max-w-7xl mx-auto space-y-4 md:space-y-0">
            <span class="font-bold text-[#313030] dark:text-[#fcf9f8] font-headline">CRAWLBOY</span>
            <div class="flex flex-wrap justify-center gap-8 font-mono text-xs uppercase tracking-widest">
                <a href="https://github.com/aksharahegde/crawlboy" target="_blank" rel="noopener noreferrer" class="text-[#313030] dark:text-[#fcf9f8] opacity-50 hover:text-primary dark:hover:text-[#00ff41] transition-colors">GitHub</a>
                <a href="https://github.com/aksharahegde/crawlboy#readme" target="_blank" rel="noopener noreferrer" class="text-[#313030] dark:text-[#fcf9f8] opacity-50 hover:text-primary dark:hover:text-[#00ff41] transition-colors">Docs</a>
                <a href="https://pypi.org/project/crawlboy/" target="_blank" rel="noopener noreferrer" class="text-[#313030] dark:text-[#fcf9f8] opacity-50 hover:text-primary dark:hover:text-[#00ff41] transition-colors">PyPI</a>
                <a href="https://github.com/aksharahegde/crawlboy/blob/main/LICENSE" target="_blank" rel="noopener noreferrer" class="text-[#313030] dark:text-[#fcf9f8] opacity-50 hover:text-primary dark:hover:text-[#00ff41] transition-colors">License</a>
            </div>
            <div class="font-mono text-xs uppercase tracking-widest text-[#313030] dark:text-[#fcf9f8] opacity-50">
                Built by <a href="https://www.aksharahegde.xyz" target="_blank" rel="noopener noreferrer" class="text-primary dark:text-[#00ff41] hover:underline">Akshara Hegde</a>
            </div>
        </div>
    </footer>

    <script>
        // Theme toggle
        const toggle = document.getElementById('theme-toggle');
        const html = document.documentElement;
        function setTheme(dark) {
            html.classList.toggle('dark', dark);
            html.classList.toggle('light', !dark);
            toggle.textContent = dark ? 'dark_mode' : 'light_mode';
            localStorage.setItem('theme', dark ? 'dark' : 'light');
        }
        toggle.addEventListener('click', () => setTheme(!html.classList.contains('dark')));
        // Init from localStorage or system preference
        const saved = localStorage.getItem('theme');
        if (saved) { setTheme(saved === 'dark'); }
        else { setTheme(window.matchMedia('(prefers-color-scheme: dark)').matches); }

        // Scroll reveal
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); observer.unobserve(e.target); } });
        }, { threshold: 0.1, rootMargin: '0px 0px -80px 0px' });
        document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

        // Smooth scroll
        document.querySelectorAll('a[href^="#"]').forEach(a => {
            a.addEventListener('click', e => {
                e.preventDefault();
                const target = document.querySelector(a.getAttribute('href'));
                if (target) target.scrollIntoView({ behavior: 'smooth', block: 'start' });
            });
        });
    </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html` in a browser. Verify:
- Nav renders with glassmorphic blur
- Theme toggle switches between light/dark
- Footer links all point to correct URLs
- "Install" button scrolls (no target yet — will be added)

---

## Task 2: Hero section — bold headline + animated terminal

**Files:**
- Modify: `index.html` (add hero inside `<main>`)

- [ ] **Step 1: Add the hero section**

Inside `<main>`, replace the placeholder comment with the hero. This uses alt.html's 2-column layout with alt_2.html's bold typography, plus a new animated terminal demo that types real crawlboy commands.

```html
<!-- HERO -->
<section class="max-w-7xl mx-auto px-8 grid grid-cols-1 lg:grid-cols-12 gap-12 items-center mb-32">
    <div class="lg:col-span-6 reveal">
        <span class="font-mono text-xs uppercase tracking-widest text-primary mb-4 block">Sitemap Crawler for Python</span>
        <h1 class="text-5xl md:text-7xl font-headline font-bold tracking-tighter leading-[0.95] text-on-surface dark:text-[#fcf9f8] mb-8">
            Crawl sitemaps.<br/>
            <span class="text-primary dark:text-primary-container">Get markdown.</span>
        </h1>
        <p class="text-lg text-secondary dark:text-[#a0a0a0] max-w-xl leading-relaxed mb-10">
            Sequentially crawl every URL from a sitemap using Crawl4AI. One markdown file per page, directory structure mirrored from your URL paths.
        </p>
        <div class="flex flex-col sm:flex-row gap-4 items-start sm:items-center">
            <div class="bg-surface-container-highest dark:bg-[#3b3a3a] px-5 py-3 flex items-center gap-3 group">
                <span class="font-mono text-secondary dark:text-[#888] select-none">$</span>
                <code class="font-mono text-on-surface dark:text-primary-container font-medium">pip install crawlboy</code>
                <button onclick="copyText('pip install crawlboy', this)" class="material-symbols-outlined text-primary hover:text-primary-container transition-colors text-xl cursor-pointer" title="Copy">content_copy</button>
            </div>
            <a href="https://github.com/aksharahegde/crawlboy#readme" target="_blank" rel="noopener noreferrer" class="text-on-surface dark:text-[#fcf9f8] font-headline font-bold text-sm tracking-widest uppercase flex items-center gap-2 group hover:text-primary transition-colors">
                Documentation
                <span class="material-symbols-outlined transition-transform group-hover:translate-x-1 text-lg">arrow_forward</span>
            </a>
        </div>
    </div>
    <div class="lg:col-span-6 relative reveal reveal-delay-2">
        <!-- Animated Terminal -->
        <div class="bg-inverse-surface dark:bg-[#1a1a1a] rounded-md overflow-hidden shadow-2xl border border-outline-variant/10 dark:border-[#333]" id="hero-terminal">
            <div class="bg-[#3b3a3a] dark:bg-[#111] px-4 py-2 flex items-center justify-between border-b border-surface/5">
                <span class="font-mono text-[10px] text-[#888] uppercase tracking-widest">process: crawl-init</span>
                <div class="flex gap-1.5">
                    <div class="w-2.5 h-2.5 rounded-full bg-[#555]"></div>
                    <div class="w-2.5 h-2.5 rounded-full bg-[#555]"></div>
                </div>
            </div>
            <div class="p-6 font-mono text-sm leading-relaxed min-h-[320px]" id="terminal-output">
                <!-- JS will populate this -->
            </div>
        </div>
        <div class="absolute -z-10 -bottom-4 -right-4 w-full h-full bg-primary/5 dark:bg-primary/10 rounded-md"></div>
    </div>
</section>
```

- [ ] **Step 2: Add the terminal typing animation JavaScript**

Add this before the closing `</script>` tag (inside the existing script block):

```javascript
// Terminal typing animation
(function() {
    const lines = [
        { text: '➜ crawlboy https://example.com/sitemap.xml --out-dir ./out', type: 'command', delay: 0 },
        { text: '', type: 'blank', delay: 800 },
        { text: '[09:44:01] INITIALIZING ENGINE...', type: 'log', delay: 1200 },
        { text: '[09:44:01] TARGET: example.com', type: 'log', delay: 1800 },
        { text: '[09:44:02] FETCHING SITEMAP... 200 OK', type: 'log', delay: 2400 },
        { text: '[09:44:02] DISCOVERED 6 URLS', type: 'log', delay: 3000 },
        { text: '', type: 'blank', delay: 3400 },
        { text: 'FOUND  /about-us', type: 'found', delay: 3800 },
        { text: 'FOUND  /blog/getting-started', type: 'found', delay: 4200 },
        { text: 'FOUND  /docs/api-reference', type: 'found', delay: 4600 },
        { text: 'FOUND  /pricing', type: 'found', delay: 5000 },
        { text: '', type: 'blank', delay: 5400 },
        { text: '→ out/md/about-us.md', type: 'output', delay: 5800 },
        { text: '→ out/md/blog/getting-started.md', type: 'output', delay: 6200 },
        { text: '→ out/md/docs/api-reference.md', type: 'output', delay: 6600 },
        { text: '→ out/md/pricing.md', type: 'output', delay: 7000 },
        { text: '', type: 'blank', delay: 7400 },
        { text: 'SUCCESS: 6 pages → markdown in 2.1s', type: 'success', delay: 7800 },
    ];
    const container = document.getElementById('terminal-output');
    const colorMap = {
        command: 'text-primary-container',
        log: 'text-[#888]',
        found: '', // split coloring below
        output: 'text-[#ccc]',
        success: 'text-primary-container font-bold',
        blank: '',
    };
    function render() {
        container.innerHTML = '';
        lines.forEach((line, i) => {
            const div = document.createElement('div');
            div.style.opacity = '0';
            div.style.transition = 'opacity 0.4s ease';
            if (line.type === 'blank') {
                div.innerHTML = '&nbsp;';
            } else if (line.type === 'found') {
                div.innerHTML = `<span class="text-primary-container">FOUND</span>  <span class="text-[#fcf9f8]">${line.text.replace('FOUND  ', '')}</span>`;
            } else {
                div.className = colorMap[line.type] || '';
                div.textContent = line.text;
            }
            container.appendChild(div);
            setTimeout(() => { div.style.opacity = '1'; }, line.delay);
        });
        // Add blinking cursor at end
        const cursor = document.createElement('div');
        cursor.className = 'text-primary-container cursor-blink mt-2';
        cursor.textContent = '_';
        cursor.style.opacity = '0';
        container.appendChild(cursor);
        setTimeout(() => { cursor.style.opacity = '1'; }, 8200);
        // Restart after pause
        setTimeout(render, 14000);
    }
    // Start when hero terminal is in view
    const termObs = new IntersectionObserver((entries) => {
        if (entries[0].isIntersecting) { render(); termObs.disconnect(); }
    });
    termObs.observe(document.getElementById('hero-terminal'));
})();

// Copy to clipboard
function copyText(text, btn) {
    navigator.clipboard.writeText(text).then(() => {
        btn.textContent = 'check';
        setTimeout(() => { btn.textContent = 'content_copy'; }, 1500);
    });
}
```

- [ ] **Step 3: Verify in browser**

Open in browser. Verify:
- Hero renders 2-column on desktop, stacks on mobile
- Terminal animation plays through all lines with staggered fade-in
- Blinking cursor appears after final line
- Animation restarts after ~14 seconds
- Copy button changes to checkmark on click
- Light/dark theme both look correct

---

## Task 3: Features section — 3-column grid with real features

**Files:**
- Modify: `index.html` (add after hero, inside `<main>`)

- [ ] **Step 1: Add the features section**

Add this after the hero section closing `</section>`:

```html
<!-- FEATURES -->
<section id="features" class="max-w-7xl mx-auto px-8 mb-32">
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-12">
        <div class="lg:col-span-4 reveal">
            <span class="font-mono text-xs uppercase tracking-widest text-primary mb-4 block">Capabilities</span>
            <h2 class="font-headline text-4xl font-bold tracking-tight text-on-surface dark:text-[#fcf9f8]">
                Why <span class="text-primary dark:text-primary-container">Crawlboy</span>?
            </h2>
            <p class="mt-6 text-secondary dark:text-[#a0a0a0] leading-relaxed">
                A focused tool that does one thing well — crawl sitemaps and produce clean, structured markdown.
            </p>
        </div>
        <div class="lg:col-span-8 grid grid-cols-1 md:grid-cols-3 gap-px bg-outline-variant/10 dark:bg-[#333]/30">
            <div class="bg-surface dark:bg-[#1c1b1b] p-8 reveal reveal-delay-1 group">
                <div class="w-12 h-12 bg-surface-container-low dark:bg-[#333] flex items-center justify-center mb-6 rounded-lg transition-colors group-hover:bg-primary-container">
                    <span class="material-symbols-outlined text-primary">explore</span>
                </div>
                <h3 class="font-headline text-xl font-bold mb-3">Sitemap Discovery</h3>
                <p class="text-sm text-secondary dark:text-[#a0a0a0] leading-relaxed mb-4">Auto-detect sitemaps from robots.txt or common paths. Recursively follows nested sitemap indexes.</p>
                <code class="font-mono text-[10px] bg-inverse-surface text-primary-container px-3 py-2 rounded inline-block">$ crawlboy --site-url example.com</code>
            </div>
            <div class="bg-surface-container-low dark:bg-[#252525] p-8 reveal reveal-delay-2 group">
                <div class="w-12 h-12 bg-surface-container-highest dark:bg-[#333] flex items-center justify-center mb-6 rounded-lg transition-colors group-hover:bg-primary-container">
                    <span class="material-symbols-outlined text-primary">description</span>
                </div>
                <h3 class="font-headline text-xl font-bold mb-3">Markdown Output</h3>
                <p class="text-sm text-secondary dark:text-[#a0a0a0] leading-relaxed mb-4">One .md file per page. Directory structure mirrors your URL paths exactly.</p>
                <code class="font-mono text-[10px] bg-inverse-surface text-primary-container px-3 py-2 rounded inline-block">/blog/post → md/blog/post.md</code>
            </div>
            <div class="bg-surface dark:bg-[#1c1b1b] p-8 reveal reveal-delay-3 group">
                <div class="w-12 h-12 bg-surface-container-low dark:bg-[#333] flex items-center justify-center mb-6 rounded-lg transition-colors group-hover:bg-primary-container">
                    <span class="material-symbols-outlined text-primary">image</span>
                </div>
                <h3 class="font-headline text-xl font-bold mb-3">Image Download</h3>
                <p class="text-sm text-secondary dark:text-[#a0a0a0] leading-relaxed mb-4">Save images to media/ (content-addressed, deduplicated). Paths auto-rewritten in output.</p>
                <code class="font-mono text-[10px] bg-inverse-surface text-primary-container px-3 py-2 rounded inline-block">$ crawlboy --download-images</code>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify in browser**

Check that:
- 3 feature cards render side by side on desktop
- Tonal shifts (not borders) separate the cards
- Icon background changes to green on hover
- Inline code snippets show real commands
- Scroll reveal animates cards with stagger

---

## Task 4: Live Demo — full-width interactive terminal session

**Files:**
- Modify: `index.html` (add after features)

- [ ] **Step 1: Add the demo section**

This is a full-width dark section showing a realistic interactive terminal output — styled like the hero terminal but larger. It shows the `--interactive` wizard flow.

```html
<!-- LIVE DEMO -->
<section id="demo" class="bg-inverse-surface dark:bg-[#111] py-24 mb-32">
    <div class="max-w-7xl mx-auto px-8">
        <div class="flex flex-col lg:flex-row items-end gap-12">
            <div class="w-full lg:w-2/3 reveal">
                <div class="bg-[#3b3a3a] dark:bg-[#0a0a0a] px-4 py-2 flex items-center justify-between rounded-t-md">
                    <span class="font-mono text-[10px] text-[#888] uppercase tracking-widest">process: interactive-mode</span>
                    <div class="flex gap-1.5">
                        <div class="w-2.5 h-2.5 rounded-full bg-[#555]"></div>
                        <div class="w-2.5 h-2.5 rounded-full bg-[#555]"></div>
                    </div>
                </div>
                <div class="bg-[#2a2a2a] dark:bg-[#0f0f0f] p-8 font-mono text-sm leading-relaxed rounded-b-md">
                    <div class="text-primary-container">➜ crawlboy <span class="text-[#fcf9f8]">--interactive</span></div>
                    <div class="mt-4 text-[#888]">
                        <p>crawlboy — Crawl sitemap URLs with Crawl4AI</p>
                        <p>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</p>
                    </div>
                    <div class="mt-4 text-[#ccc]">
                        <p class="text-[#888]">? How do you want to provide URLs?</p>
                        <p class="text-primary-container">> Discover sitemap from a site</p>
                        <p class="text-[#666]">&nbsp;&nbsp;Use a direct sitemap URL</p>
                    </div>
                    <div class="mt-4 text-[#ccc]">
                        <p class="text-[#888]">? Site origin:</p>
                        <p class="text-primary-container">https://www.example.com</p>
                    </div>
                    <div class="mt-4 text-[#ccc]">
                        <p class="text-[#888]">? Output directory:</p>
                        <p class="text-primary-container">./out</p>
                    </div>
                    <div class="mt-4 text-[#ccc]">
                        <p class="text-[#888]">? Options:</p>
                        <p><span class="text-primary-container">✓</span> Run browser headless</p>
                        <p><span class="text-primary-container">✓</span> Download images to media/</p>
                    </div>
                    <div class="mt-6 border-t border-[#444] pt-4">
                        <p class="text-primary-container">✓ Mode: <span class="text-[#fcf9f8]">site</span></p>
                        <p class="text-primary-container">✓ URL: <span class="text-[#fcf9f8]">https://www.example.com</span></p>
                        <p class="text-primary-container">✓ Out: <span class="text-[#fcf9f8]">./out</span></p>
                    </div>
                    <div class="mt-4 text-primary-container cursor-blink">_</div>
                </div>
            </div>
            <div class="w-full lg:w-1/3 pb-8 reveal reveal-delay-2">
                <span class="font-mono text-xs text-primary-container tracking-widest uppercase mb-4 block">Interactive Mode</span>
                <h2 class="font-headline text-4xl font-bold text-[#fcf9f8] leading-tight mb-6">
                    Guided setup,<br/>zero guesswork.
                </h2>
                <p class="text-[#a0a0a0] text-sm leading-relaxed mb-8">
                    The interactive wizard walks you through URL mode, output directory, crawl options, and advanced settings before confirming. Built with questionary and Rich.
                </p>
                <code class="font-mono text-xs text-primary-container">$ crawlboy -i</code>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify in browser**

Check:
- Dark full-width band with asymmetric layout (terminal left, text right)
- Terminal shows realistic `--interactive` wizard flow
- Blinking cursor at bottom of terminal
- Text right column shows explanation with real command

---

## Task 5: How It Works — output structure visualization

**Files:**
- Modify: `index.html` (add after demo)

- [ ] **Step 1: Add the output structure section**

This section shows the URL-to-file mapping with a visual tree. Asymmetric layout: tree left, explanation right.

```html
<!-- HOW IT WORKS -->
<section class="max-w-7xl mx-auto px-8 mb-32">
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-16 items-center">
        <div class="reveal">
            <div class="bg-surface-container-low dark:bg-[#252525] p-8 rounded-lg font-mono text-sm leading-loose">
                <p class="text-[#888] text-xs uppercase tracking-widest mb-6">Output Structure</p>
                <div class="text-on-surface dark:text-[#ccc]">
                    <p>out/</p>
                    <p class="pl-4">├── <span class="text-primary dark:text-primary-container">md/</span></p>
                    <p class="pl-8">├── <span class="text-on-surface dark:text-[#fcf9f8]">index.md</span> <span class="text-[#888] text-xs ml-2">← /</span></p>
                    <p class="pl-8">├── about-us.md <span class="text-[#888] text-xs ml-2">← /about-us</span></p>
                    <p class="pl-8">├── <span class="text-primary dark:text-primary-container">blog/</span></p>
                    <p class="pl-12">├── getting-started.md <span class="text-[#888] text-xs ml-2">← /blog/getting-started</span></p>
                    <p class="pl-12">└── advanced-config.md <span class="text-[#888] text-xs ml-2">← /blog/advanced-config</span></p>
                    <p class="pl-8">└── <span class="text-primary dark:text-primary-container">docs/</span></p>
                    <p class="pl-12">└── api-reference.md <span class="text-[#888] text-xs ml-2">← /docs/api-reference</span></p>
                    <p class="pl-4">├── <span class="text-primary dark:text-primary-container">html/</span> <span class="text-[#888] text-xs ml-2">--save-html</span></p>
                    <p class="pl-4">├── <span class="text-primary dark:text-primary-container">media/</span> <span class="text-[#888] text-xs ml-2">--download-images</span></p>
                    <p class="pl-4">└── errors.jsonl</p>
                </div>
            </div>
        </div>
        <div class="reveal reveal-delay-2">
            <span class="font-mono text-xs uppercase tracking-widest text-primary mb-4 block">Structured Output</span>
            <h2 class="font-headline text-4xl font-bold tracking-tight mb-6">
                URL paths become<br/>file paths.
            </h2>
            <div class="space-y-6 text-secondary dark:text-[#a0a0a0]">
                <p>Every crawled page produces a markdown file. The directory structure mirrors the URL hierarchy — no flat dump of numbered files.</p>
                <p>Optionally save raw HTML alongside markdown with <code class="font-mono text-xs bg-inverse-surface text-primary-container px-2 py-1 rounded">--save-html</code>, or download and deduplicate images with <code class="font-mono text-xs bg-inverse-surface text-primary-container px-2 py-1 rounded">--download-images</code>.</p>
                <p>Failures are logged to <code class="font-mono text-xs bg-inverse-surface text-primary-container px-2 py-1 rounded">errors.jsonl</code> — the crawl keeps going.</p>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify**

Confirm tree renders as monospace structure with green folder names and gray URL annotations.

---

## Task 6: Quick Start — tabbed installation with copy buttons

**Files:**
- Modify: `index.html` (add after how-it-works)

- [ ] **Step 1: Add the installation section**

```html
<!-- INSTALL -->
<section id="install" class="max-w-7xl mx-auto px-8 mb-32">
    <div class="max-w-3xl reveal">
        <span class="font-mono text-xs uppercase tracking-widest text-primary mb-4 block">Get Started</span>
        <h2 class="font-headline text-4xl font-bold tracking-tight mb-8">Installation</h2>
        <div class="flex gap-2 mb-6">
            <button onclick="switchInstallTab('pip')" class="install-tab active font-mono text-xs uppercase tracking-widest px-4 py-2 transition-colors cursor-pointer" data-tab="pip">pip</button>
            <button onclick="switchInstallTab('source')" class="install-tab font-mono text-xs uppercase tracking-widest px-4 py-2 transition-colors cursor-pointer" data-tab="source">Source</button>
            <button onclick="switchInstallTab('docker')" class="install-tab font-mono text-xs uppercase tracking-widest px-4 py-2 transition-colors cursor-pointer" data-tab="docker">Docker</button>
        </div>
        <div id="tab-pip" class="install-panel">
            <div class="bg-inverse-surface rounded-md overflow-hidden">
                <div class="bg-[#3b3a3a] px-4 py-2 flex items-center justify-between">
                    <span class="font-mono text-[10px] text-[#888] uppercase tracking-widest">shell</span>
                    <button onclick="copyText('pip install crawlboy\ncrawl4ai-setup', this)" class="material-symbols-outlined text-[#888] hover:text-primary-container text-sm cursor-pointer transition-colors">content_copy</button>
                </div>
                <div class="p-6 font-mono text-sm">
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">pip install</span> <span class="text-[#fcf9f8]">crawlboy</span></p>
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">crawl4ai-setup</span></p>
                </div>
            </div>
            <p class="text-xs text-secondary dark:text-[#888] mt-4">crawl4ai-setup installs Playwright/Chromium. If something fails, run <code class="font-mono text-primary dark:text-primary-container">crawl4ai-doctor</code>.</p>
        </div>
        <div id="tab-source" class="install-panel hidden">
            <div class="bg-inverse-surface rounded-md overflow-hidden">
                <div class="bg-[#3b3a3a] px-4 py-2 flex items-center justify-between">
                    <span class="font-mono text-[10px] text-[#888] uppercase tracking-widest">shell</span>
                    <button onclick="copyText('git clone https://github.com/aksharahegde/crawlboy.git\ncd crawlboy\npip install -e .\ncrawl4ai-setup', this)" class="material-symbols-outlined text-[#888] hover:text-primary-container text-sm cursor-pointer transition-colors">content_copy</button>
                </div>
                <div class="p-6 font-mono text-sm space-y-1">
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">git clone</span> <span class="text-[#fcf9f8]">https://github.com/aksharahegde/crawlboy.git</span></p>
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">cd</span> <span class="text-[#fcf9f8]">crawlboy</span></p>
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">pip install -e .</span></p>
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">crawl4ai-setup</span></p>
                </div>
            </div>
        </div>
        <div id="tab-docker" class="install-panel hidden">
            <div class="bg-inverse-surface rounded-md overflow-hidden">
                <div class="bg-[#3b3a3a] px-4 py-2 flex items-center justify-between">
                    <span class="font-mono text-[10px] text-[#888] uppercase tracking-widest">shell</span>
                    <button onclick="copyText(`docker build -t crawlboy .\ndocker run --rm -v \"$(pwd)/out:/out\" crawlboy \\\n  --site-url 'https://www.example.com' --out-dir /out`, this)" class="material-symbols-outlined text-[#888] hover:text-primary-container text-sm cursor-pointer transition-colors">content_copy</button>
                </div>
                <div class="p-6 font-mono text-sm space-y-1">
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">docker build -t</span> <span class="text-[#fcf9f8]">crawlboy .</span></p>
                    <p><span class="text-primary-container opacity-50">$</span> <span class="text-primary-container">docker run</span> <span class="text-[#fcf9f8]">--rm -v "$(pwd)/out:/out" crawlboy \</span></p>
                    <p class="pl-4"><span class="text-[#fcf9f8]">--site-url 'https://www.example.com' --out-dir /out</span></p>
                </div>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Add tab switching CSS and JavaScript**

Add this CSS inside the existing `<style>` block:

```css
.install-tab { background: transparent; color: #5f5e5e; border: none; }
.install-tab.active { background: #006e16; color: #fff; }
.dark .install-tab { color: #888; }
.dark .install-tab.active { background: #00ff41; color: #000; }
```

Add this JavaScript inside the existing `<script>` block:

```javascript
function switchInstallTab(tab) {
    document.querySelectorAll('.install-panel').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.install-tab').forEach(t => t.classList.remove('active'));
    document.getElementById('tab-' + tab).classList.remove('hidden');
    document.querySelector(`[data-tab="${tab}"]`).classList.add('active');
}
```

- [ ] **Step 3: Verify**

Check tab switching works, copy buttons work, all three install methods show correct commands.

---

## Task 7: CLI Reference — striped flag table with real flags

**Files:**
- Modify: `index.html` (add after install)

- [ ] **Step 1: Add the CLI reference section**

```html
<!-- CLI REFERENCE -->
<section id="cli" class="max-w-7xl mx-auto px-8 mb-32">
    <div class="reveal">
        <span class="font-mono text-xs uppercase tracking-widest text-primary mb-4 block">Reference</span>
        <h2 class="font-headline text-4xl font-bold tracking-tight mb-8">CLI Flags</h2>
    </div>
    <div class="bg-surface-container-low dark:bg-[#252525] overflow-hidden rounded-lg reveal reveal-delay-1">
        <div class="grid grid-cols-12 p-5 font-mono text-[10px] uppercase tracking-widest text-secondary dark:text-[#888] opacity-70">
            <div class="col-span-3">Flag</div>
            <div class="col-span-7">Description</div>
            <div class="col-span-2">Default</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-lowest dark:bg-[#1c1b1b] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--sitemap-url</div>
            <div class="col-span-7 text-sm">Direct sitemap URL</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">—</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-low dark:bg-[#252525] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--site-url</div>
            <div class="col-span-7 text-sm">Site root for auto-discovery</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">—</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-lowest dark:bg-[#1c1b1b] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--out-dir</div>
            <div class="col-span-7 text-sm">Output directory</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">—</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-low dark:bg-[#252525] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--save-html</div>
            <div class="col-span-7 text-sm">Write raw HTML under html/</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">off</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-lowest dark:bg-[#1c1b1b] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--download-images</div>
            <div class="col-span-7 text-sm">Save images under media/ and rewrite paths</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">off</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-low dark:bg-[#252525] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--delay</div>
            <div class="col-span-7 text-sm">Seconds to wait after each page</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">0</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-lowest dark:bg-[#1c1b1b] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--max-urls</div>
            <div class="col-span-7 text-sm">Cap number of URLs (for testing)</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">unlimited</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-low dark:bg-[#252525] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">--fail-fast</div>
            <div class="col-span-7 text-sm">Stop on first crawl error</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">off</div>
        </div>
        <div class="grid grid-cols-12 p-5 bg-surface-container-lowest dark:bg-[#1c1b1b] hover:bg-primary-container/10 transition-colors">
            <div class="col-span-3 font-mono text-primary dark:text-primary-container font-bold text-sm">-i, --interactive</div>
            <div class="col-span-7 text-sm">Launch guided wizard</div>
            <div class="col-span-2 font-mono text-sm text-secondary dark:text-[#888]">off</div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify**

Confirm all 9 flags match the README. Rows alternate backgrounds. Hover highlights in green.

---

## Task 8: Bottom CTA section

**Files:**
- Modify: `index.html` (add after CLI reference, before closing `</main>`)

- [ ] **Step 1: Add the CTA**

```html
<!-- CTA -->
<section class="bg-inverse-surface dark:bg-[#111] py-24 text-center">
    <div class="max-w-4xl mx-auto px-8 reveal">
        <h2 class="font-headline text-4xl font-bold text-[#fcf9f8] mb-8">Ready to start crawling?</h2>
        <div class="inline-block bg-[#fcf9f8]/5 p-2 rounded-lg">
            <div class="bg-[#fcf9f8]/10 px-8 py-6 flex flex-col md:flex-row items-center gap-6">
                <code class="font-mono text-primary-container text-xl">$ pip install crawlboy</code>
                <a href="https://github.com/aksharahegde/crawlboy" target="_blank" rel="noopener noreferrer" class="bg-primary-container text-[#000] px-10 py-4 font-headline font-bold uppercase tracking-widest text-sm hover:bg-primary-fixed transition-colors inline-block">
                    Get Started
                </a>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify**

Dark band, centered content, install command + green button linking to GitHub.

---

## Task 9: Final polish — re-register scroll observers and test everything

**Files:**
- Modify: `index.html` (adjust script)

- [ ] **Step 1: Move observer registration to end of script**

The scroll reveal observer (`document.querySelectorAll('.reveal')...`) must run after all sections are in the DOM. Ensure it's the last thing in the `<script>` block. The current scaffold already has it at the bottom, but verify it's after all function definitions.

- [ ] **Step 2: Full test checklist**

Test in browser:
1. **Light theme**: all sections readable, correct backgrounds, green accents visible
2. **Dark theme**: toggle, verify all sections have dark backgrounds, text is visible, terminal glows
3. **Hero terminal**: animation plays, restarts after pause, cursor blinks
4. **Nav links**: #features, #demo, #install, #cli all scroll to correct sections
5. **Install tabs**: pip/source/docker switch correctly
6. **Copy buttons**: all 4 copy buttons (hero, pip, source, docker) produce checkmark feedback
7. **Footer links**: GitHub, Docs, PyPI, License all open correct URLs in new tabs
8. **"Built by Akshara Hegde"**: links to aksharahegde.xyz
9. **Mobile**: nav collapses (links hidden), sections stack vertically, terminal readable
10. **Content accuracy**: no fabricated features, flags, or commands anywhere on the page
