# GitHub Profile README Redesign — Apple Minimal + Rich Animation

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Redesign anomredux GitHub profile README with Apple-inspired black/white minimal aesthetic, maximizing markdown animation capabilities.

**Architecture:** Custom animated SVGs with internal CSS `@keyframes` for hero/section animations, `<picture>` tag throughout for dark/light auto-switching, monochrome skill badges, and Platane/snk contribution snake. All content centered with generous whitespace.

**Tech Stack:** Markdown, inline HTML, custom SVG with CSS animations, GitHub Actions (Platane/snk), shields.io, github-readme-stats, readme-typing-svg

---

## Design Direction

### Apple Aesthetic Principles
- **Color**: Pure black `#000` / white `#fff` only. Gray `#888` for secondary text. No color badges.
- **Typography**: SF Pro Display feel — thin weight (100-300), large sizes, tracking (letter-spacing)
- **Spacing**: Extreme whitespace. Every section breathes. Multiple `<br>` between sections.
- **Animation**: Smooth, deliberate. Fade-ins, slide-ups. Like Apple product reveal pages.
- **Content**: Less is more. Only essential information. Let the work speak.

### Dark/Light Mode Strategy
Every visual element uses `<picture>` with `prefers-color-scheme`:
- **Light mode**: Black text on white/transparent background
- **Dark mode**: White text on dark/transparent background

### Section Layout

```
┌─────────────────────────────────────────────┐
│                                             │
│          [Custom Animated SVG Hero]         │
│          Name + Title fade-in               │
│          Typing SVG tagline                 │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│              About (2-3 lines)              │
│         Minimal, Apple-keynote style        │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│         Tech Stack (monochrome icons)       │
│     skillicons.dev dark/light + custom      │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│         GitHub Stats (transparent bg)       │
│     Stats card + Streak (monochrome)        │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│          Contribution Snake 🐍              │
│     (already working, keep as-is)           │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│               Links                         │
│    mghong.dev  ·  blog  ·  email            │
│        (monochrome flat badges)             │
│                                             │
├─────────────────────────────────────────────┤
│                                             │
│          visitor counter (minimal)          │
│                                             │
└─────────────────────────────────────────────┘
```

---

## Task 1: Create Hero Animation SVG (Light Mode)

**Files:**
- Create: `assets/hero-light.svg`

**Step 1: Create assets directory**

```bash
mkdir -p assets
```

**Step 2: Create hero SVG with CSS animations**

The hero SVG will feature:
- Name "Mingi Hong" fading in with a slight upward slide (Apple product reveal)
- Subtitle "Machine Learning Engineer" fading in with delay
- A thin horizontal line drawing itself across (like Apple's section dividers)
- All in black on transparent background

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 220" fill="none">
  <style>
    @keyframes fadeSlideUp {
      0% { opacity: 0; transform: translateY(20px); }
      100% { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      0% { opacity: 0; }
      100% { opacity: 1; }
    }
    @keyframes drawLine {
      0% { stroke-dashoffset: 400; }
      100% { stroke-dashoffset: 0; }
    }
    .name {
      font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Helvetica Neue', sans-serif;
      font-size: 52px;
      font-weight: 200;
      fill: #000;
      opacity: 0;
      animation: fadeSlideUp 1.2s ease-out 0.3s forwards;
    }
    .title {
      font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Helvetica Neue', sans-serif;
      font-size: 18px;
      font-weight: 300;
      fill: #555;
      letter-spacing: 4px;
      text-transform: uppercase;
      opacity: 0;
      animation: fadeIn 1.5s ease-out 1.2s forwards;
    }
    .line {
      stroke: #000;
      stroke-width: 0.5;
      stroke-dasharray: 400;
      stroke-dashoffset: 400;
      animation: drawLine 1.5s ease-in-out 2s forwards;
    }
  </style>
  <text x="400" y="100" text-anchor="middle" class="name">Mingi Hong</text>
  <text x="400" y="140" text-anchor="middle" class="title">Machine Learning Engineer</text>
  <line x1="200" y1="175" x2="600" y2="175" class="line" />
</svg>
```

---

## Task 2: Create Hero Animation SVG (Dark Mode)

**Files:**
- Create: `assets/hero-dark.svg`

**Step 1: Create dark variant**

Same as light but with color inversions:
- Name fill: `#f0f0f0`
- Title fill: `#999`
- Line stroke: `#f0f0f0`

---

## Task 3: Update Snake Workflow for Monochrome Style

**Files:**
- Modify: `.github/workflows/snake.yml`

**Step 1: Review current workflow**

Current workflow already generates light and dark variants. Consider customizing colors to be monochrome:

```yaml
name: Generate Snake Animation

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg?palette=github-dark
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

The default palette already works well with the minimal aesthetic. The snake animation is the one "rich" animation element — it provides movement and life to an otherwise still page (Apple keynote-style contrast between calm and dynamic).

**Decision**: Keep current workflow as-is. The default GitHub palette is already grayscale-ish and fits the minimal theme.

---

## Task 4: Create Animated Section Divider SVG

**Files:**
- Create: `assets/divider-light.svg`
- Create: `assets/divider-dark.svg`

**Step 1: Create a minimal line-draw divider**

A thin line that draws itself from center outward — used between sections for Apple-style visual rhythm.

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 20" fill="none">
  <style>
    @keyframes expandLine {
      0% { stroke-dashoffset: 200; }
      100% { stroke-dashoffset: 0; }
    }
    .divider {
      stroke: #d1d1d1;
      stroke-width: 0.5;
      stroke-dasharray: 200;
      stroke-dashoffset: 200;
      animation: expandLine 1.5s ease-in-out forwards;
    }
  </style>
  <line x1="200" y1="10" x2="600" y2="10" class="divider" />
</svg>
```

Dark variant: stroke `#333`.

---

## Task 5: Compose the README.md

**Files:**
- Modify: `README.md`

**Step 1: Write the complete README**

Structure with generous whitespace, `<picture>` tags for all assets, and Apple-minimal styling:

```markdown
<div align="center">

<br>
<br>

<!-- Hero: Animated name + title -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/hero-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="./assets/hero-light.svg" />
  <img alt="Mingi Hong — Machine Learning Engineer" src="./assets/hero-light.svg" width="800" />
</picture>

<br>

<!-- Typing tagline -->
[![Typing SVG](https://readme-typing-svg.demolab.com?font=Helvetica+Neue&weight=300&size=16&duration=3000&pause=2000&color=888888&center=true&vCenter=true&repeat=true&width=500&height=30&lines=Shipping+ML+to+production.;From+models+to+infrastructure.;Building+systems+that+scale.)](https://git.io/typing-svg)

<br>
<br>
<br>

<!-- About -->
<sub>
Building production ML systems for computational pathology at MTSCO.<br>
Research in diffusion models. Infrastructure with Go and Kubernetes.<br>
M.S. Artificial Intelligence, Hanyang University.
</sub>

<br>
<br>
<br>

<!-- Divider -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/divider-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="./assets/divider-light.svg" />
  <img alt="" src="./assets/divider-light.svg" width="800" />
</picture>

<br>
<br>

<!-- Tech Stack: skillicons with dark/light -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://skillicons.dev/icons?i=python,go,pytorch,docker,kubernetes,fastapi&theme=dark" />
  <source media="(prefers-color-scheme: light)" srcset="https://skillicons.dev/icons?i=python,go,pytorch,docker,kubernetes,fastapi&theme=light" />
  <img alt="Tech Stack" src="https://skillicons.dev/icons?i=python,go,pytorch,docker,kubernetes,fastapi&theme=light" />
</picture>

<br>
<br>
<br>

<!-- Divider -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/divider-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="./assets/divider-light.svg" />
  <img alt="" src="./assets/divider-light.svg" width="800" />
</picture>

<br>
<br>

<!-- GitHub Stats -->
<picture>
  <source
    media="(prefers-color-scheme: dark)"
    srcset="https://github-readme-stats.vercel.app/api?username=anomredux&show_icons=true&bg_color=00000000&title_color=ffffff&text_color=cccccc&icon_color=ffffff&hide_border=true&rank_icon=github"
  />
  <source
    media="(prefers-color-scheme: light)"
    srcset="https://github-readme-stats.vercel.app/api?username=anomredux&show_icons=true&bg_color=00000000&title_color=000000&text_color=333333&icon_color=000000&hide_border=true&rank_icon=github"
  />
  <img alt="GitHub Stats" src="https://github-readme-stats.vercel.app/api?username=anomredux&show_icons=true&hide_border=true" height="165" />
</picture>
&nbsp;&nbsp;
<picture>
  <source
    media="(prefers-color-scheme: dark)"
    srcset="https://github-readme-streak-stats.herokuapp.com/?user=anomredux&background=00000000&ring=ffffff&fire=ffffff&currStreakNum=ffffff&sideNums=ffffff&currStreakLabel=cccccc&sideLabels=cccccc&dates=888888&hide_border=true"
  />
  <source
    media="(prefers-color-scheme: light)"
    srcset="https://github-readme-streak-stats.herokuapp.com/?user=anomredux&background=00000000&ring=000000&fire=000000&currStreakNum=000000&sideNums=000000&currStreakLabel=333333&sideLabels=333333&dates=888888&hide_border=true"
  />
  <img alt="GitHub Streak" src="https://github-readme-streak-stats.herokuapp.com/?user=anomredux&hide_border=true" height="165" />
</picture>

<br>
<br>
<br>

<!-- Divider -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/divider-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="./assets/divider-light.svg" />
  <img alt="" src="./assets/divider-light.svg" width="800" />
</picture>

<br>
<br>

<!-- Snake Animation -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/anomredux/anomredux/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/anomredux/anomredux/output/github-snake.svg" />
  <img alt="contribution snake" src="https://raw.githubusercontent.com/anomredux/anomredux/output/github-snake.svg" width="800" />
</picture>

<br>
<br>
<br>

<!-- Divider -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/divider-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="./assets/divider-light.svg" />
  <img alt="" src="./assets/divider-light.svg" width="800" />
</picture>

<br>
<br>

<!-- Links -->
[![Website](https://img.shields.io/badge/mghong.dev-000000?style=flat-square&logo=safari&logoColor=white)](https://mghong.dev)
&nbsp;&nbsp;
[![Blog](https://img.shields.io/badge/blog-000000?style=flat-square&logo=hashnode&logoColor=white)](https://mghong.dev/blog)
&nbsp;&nbsp;
[![Email](https://img.shields.io/badge/me@mghong.dev-000000?style=flat-square&logo=gmail&logoColor=white)](mailto:me@mghong.dev)

<br>
<br>
<br>

<!-- Visitor Counter -->
![Visitors](https://komarev.com/ghpvc/?username=anomredux&color=000000&style=flat-square&label=visitors)

<br>

</div>
```

---

## Task 6: Verify and Refine

**Step 1: Review all SVG files render correctly**

Check that:
- SVGs load in browser locally
- Animations play (fadeSlideUp, drawLine, expandLine)
- Dark/light variants have correct colors

**Step 2: Review README markdown**

Check that:
- All `<picture>` tags have correct paths
- Typing SVG parameters match the minimal theme
- Badges are all monochrome black (`000000`)
- skillicons.dev URL has correct icon list
- Stats card URLs have correct username

**Step 3: Test dark/light mode switching**

- Open GitHub in light mode → verify light SVGs show
- Switch to dark mode → verify dark SVGs show

---

## Design Notes

### Why This Works (Apple Aesthetic)

1. **Hero animation** — Like an Apple product name reveal. Thin weight, fade + slide up, then a line draws itself. Deliberate timing with delays.

2. **Typing SVG** — Subtle, gray (#888), small size (16px), Helvetica Neue light. Not screaming — whispering. Like Apple's taglines.

3. **About section** — `<sub>` tag makes text small and understated. Three lines, no more. Apple never over-explains.

4. **Skill icons** — No colorful badges. skillicons.dev with theme switching. Clean grid, no labels.

5. **Stats** — Transparent background, monochrome colors. The data is visible but doesn't dominate.

6. **Snake** — The ONE animation that's bold and playful. Apple always has one "wow" moment. The snake eating contribution cells is it.

7. **Links** — `flat-square` style, all black. Uniform. Like Apple's footer links.

8. **Spacing** — Triple `<br>` between major sections. The page breathes. Apple uses whitespace as a design element.

### What's Different From Current README

| Before | After |
|--------|-------|
| Colorful gradient capsule-render | Custom animated SVG hero (b/w) |
| Blue typing SVG, Fira Code | Gray typing SVG, Helvetica Neue, smaller |
| Colored for-the-badge shields | Monochrome flat-square shields |
| tokyonight theme stats cards | Transparent bg, monochrome stats |
| `---` horizontal rules | Animated SVG dividers |
| Emoji section headers (🛠 📌 📫) | No emojis, no section headers |
| Placeholder project cards | Removed (add when real repos exist) |
| Capsule-render footer | Clean fade-out with visitor counter |

### Content Decisions

- **Removed Projects section**: Current placeholders (`YOUR_REPO_1`) look unprofessional. Add back when real repos exist.
- **Removed section headers**: Apple doesn't label sections. The visual hierarchy speaks for itself.
- **Added About text**: Brief, factual. Computational pathology, diffusion models, Hanyang University. Establishes credibility without boasting.
- **Blog link**: Points to mghong.dev/blog — ready for when posts exist.
